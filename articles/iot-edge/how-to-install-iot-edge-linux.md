---
title: Linux に Azure IoT Edge をインストールする方法 | Microsoft Docs
description: Linux に Azure IoT Edge をインストールする手順
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: kgremban
ms.openlocfilehash: 56223b2ed8e9d9b1a08f5313940920113a650bfe
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128334"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Linux に Azure IoT Edge ランタイムをインストールする (x64)

Azure IoT Edge ランタイムを使用すると、デバイスを IoT Edge デバイスに変えることができます。 このランタイムは、Raspberry Pi のような小型デバイスにも、産業用サーバーのような大型デバイスにもデプロイすることができます。 IoT Edge ランタイムを使用してデバイスを構成すると、クラウドからデバイスへのビジネス ロジックのデプロイを開始できます。 

IoT Edge ランタイムの動作とランタイムに含まれるコンポーネントについては、「[Azure IoT Edge ランタイムとそのアーキテクチャの概要](iot-edge-runtime.md)」を参照してください。

この記事では、Linux x64 (Intel/AMD) Edge デバイスに Azure IoT Edge ランタイムをインストールする手順を示します。 現在サポートされている AMD64 オペレーティング システムの一覧については、「[Azure IoT Edge のサポート](support.md#operating-systems)」を参照してください。 

>[!NOTE]
>Linux ソフトウェア リポジトリ内のパッケージは、各パッケージ (/usr/share/doc/*パッケージ名*) 内にあるライセンス条項の対象となります。 パッケージを使用する前に、ライセンス条項をお読みください。 インストールし、パッケージを使用すると、これらの条項に同意したものと見なされます。 ライセンス条項に同意しない場合は、パッケージを使用しないでください。

## <a name="register-microsoft-key-and-software-repository-feed"></a>Microsoft キーとソフトウェア リポジトリ フィードを登録する

オペレーティング システムに応じて、IoT Edge ランタイムのインストール用にデバイスを準備するための適切なスクリプトを選択します。 

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/

# Perform apt upgrade
sudo apt-get upgrade
```

## <a name="install-the-container-runtime"></a>コンテナー ランタイムをインストールする 

Azure IoT Edge は、[OCI と互換性のある][lnk-oci]コンテナー ランタイムに依存します。 実稼働環境シナリオでは、以下の [Moby ベース][lnk-moby] エンジンを使用することを強くお勧めします。 これは、Azure IoT Edge で公式にサポートされている唯一のコンテナー エンジンです。 Docker CE/EE コンテナー イメージは、Moby ランタイムと互換性があります。

apt-get を更新します。

```bash
sudo apt-get update
```

Moby エンジンをインストールします。 

```bash
sudo apt-get install moby-engine
```

Moby コマンドライン インターフェイス (CLI) をインストールします。 CLI は開発には役立ちますが、実稼働環境には省略可能です。

```bash
sudo apt-get install moby-cli
```

## <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IoT Edge セキュリティ デーモンのインストール

**IoT Edge セキュリティ デーモン**は、Edge デバイス上のセキュリティ標準を提供し、維持します。 デーモンは起動のたびに開始され、IoT Edge ランタイムの残りの部分を開始することでデバイスをブートストラップします。 

このインストール コマンドでは、**iothsmlib** の標準バージョンもインストールされます (まだ存在しない場合)。

apt-get を更新します。

```bash
sudo apt-get update
```

セキュリティ デーモンをインストールします。 パッケージは `/etc/iotedge/` にインストールされます。

```bash
sudo apt-get install iotedge
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Azure IoT Edge セキュリティ デーモンの構成

IoT Edge ランタイムを構成して、物理デバイスを Azure IoT ハブに存在するデバイス ID にリンクします。 

デーモンは、`/etc/iotedge/config.yaml` にある構成ファイルを使用して構成できます。 このファイルは既定で書き込み禁止になっています。編集するには管理者特権が必要な場合があります。

IoT Hub によって提供されるデバイス接続文字列を使用して、1 つの IoT Edge デバイスを手動でプロビジョニングすることもできますし、 Device Provisioning Service を使用して、複数のデバイスを自動的にプロビジョニングすることもできます。これは、プロビジョニングするデバイスが多数ある場合に便利です。 目的にプロビジョニング方法に応じて、適切なインストール スクリプトを選択してください。 

### <a name="option-1-manual-provisioning"></a>オプション 1: 手動プロビジョニング

デバイスを手動でプロビジョニングするには、[デバイス接続文字列][lnk-dcs]をデバイスに提供する必要があります。この文字列は、新しいデバイスを IoT ハブに登録することで作成できます。


構成ファイルを開きます。 

```bash
sudo nano /etc/iotedge/config.yaml
```

ファイルの provisioning セクションを見つけ、**manual** プロビジョニング モードをコメント解除します。 **device_connection_string** の値を IoT Edge デバイスからの接続文字列で更新します。

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning: 
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

ファイルを保存して閉じます。 

   `CTRL + X`、`Y`、`Enter`

構成ファイルにプロビジョニング情報を入力した後、デーモンを再起動します。

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>オプション 2: 自動プロビジョニング

デバイスを自動的にプロビジョニングするには、[Device Provisioning Service を設定し、デバイス登録 ID を取得][lnk-dps]します (DPS)。 自動プロビジョニングは、トラステッド プラットフォーム モジュール (TPM) チップが搭載されているデバイスでのみ機能します。 たとえば、Raspberry Pi デバイスには、既定で TPM が搭載されていません。 

構成ファイルを開きます。 

```bash
sudo nano /etc/iotedge/config.yaml
```

ファイルの provisioning セクションを見つけ、**dps** プロビジョニング モードをコメント解除します。 **scope_id** と **registration_id** の値を、IoT Hub Device Provisioning Service と TPM を搭載した IoT Edge デバイスの値で更新します。 

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning: 
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

ファイルを保存して閉じます。 

   `CTRL + X`、`Y`、`Enter`

構成ファイルにプロビジョニング情報を入力した後、デーモンを再起動します。

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>インストールの成功を確認する

前のセクションで**手動構成**手順を使用した場合、IoT Edge ランタイムがデバイス上で正常にプロビジョニングおよび実行されている必要があります。 **自動構成**手順を使用した場合は、ランタイムが IoT ハブにデバイスを登録できるように、追加の手順を完了する必要があります。 次の手順については、[Linux 仮想マシンでのシミュレートされた TPM Edge デバイスの作成とプロビジョニング](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm)に関する記事をご覧ください。

以下を使用して、IoT Edge デーモンの状態を確認できます。

```bash
systemctl status iotedge
```

以下を使用して、デーモンのログを確認します。

```bash
journalctl -u iotedge --no-pager --no-full
```

また、以下を使用して、実行中のモジュールを一覧表示します。

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>ヒントと検索候補

`iotedge` コマンドの実行には、昇格された特権が必要です。 ランタイムをインストールしたら、マシンからサインアウトした後サインインし直して、自動的にアクセス許可を更新します。 それまでは、すべての `iotedge` コマンドの前に **sudo** を使用します。

リソースに制約のあるデバイスでは、[トラブルシューティング ガイド][lnk-trouble]に示されているように、*OptimizeForPerformance* 環境変数を *false* に設定することを強くお勧めします。

## <a name="next-steps"></a>次の手順

Edge ランタイムを正常にインストールできない場合は、[トラブルシューティング][lnk-trouble]のページを調べてください。

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
