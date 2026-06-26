# Flexera-One---Beacon-Agent

IBM Concert v3.0 on VM のデプロイを Ansible で自動化します。

過去のバージョンをインストールする場合は任意のブランチに切り替えて実行してください。

## 前提条件
- 作業端末で Ansible が実行可能なこと。
- 仮想マシンのPublic IPおよび、SSHキーを入手していること。
  - https://techzone.ibm.com/collection/69c6c2951bdc18e8109d08ea?platform=69cae43833fe65185ca168b2
  - Geographyでは、`itz-vpc-02` シリーズを選択。(`itz-vpc-01`だとNWの問題でDeployが成功しません)
  - ![alt text](./images/image.png)
- Entitlement Keyを入手していること。
  - https://myibm.ibm.com/products-services/containerlibrary
- (オプション)あだちにコーヒー1杯をご馳走する

## 事前準備（Beacon用Windows）
- `inventory.ini` : 対象ホストの論理名（`concert_vm`）を定義
- `group_vars/concert_vm.template.yml` : 接続パラメータのテンプレート
- `group_vars/concert_vm.yml` : 実際に使う接続パラメータ（テンプレートをコピーして作成、`.gitignore` 済み）
- `playbooks/setup.yml` : 全体の Playbook（`common`, `concert` ロールを順番に実行）
- `roles/common` : firewalld無効化、DNS設定、パッケージインストール、umask/linger設定
- `roles/concert` : Concert v3.0パッケージダウンロード、manage-prereqs-vm実行、params.ini設定、Concertセットアップ実行
- `ansible.cfg` : inventory/roles_path のデフォルト設定

### v3.0での主な変更点
- **K3s管理の変更**: `manage-prereqs-vm`スクリプトがK3sとHelmをインストール（k3s roleは削除）
- **firewalld無効化**: Concert v3.0の要件に従い自動的に無効化
- **DNS設定**: IBM Container Registry (cp.icr.io) の名前解決のためGoogle DNS (8.8.8.8) を追加
- **registries.yaml自動修正**: プライベートIPが設定されている場合、パブリックIPに自動修正
- **Secure Coderサポート**: オプションでSecure Coderコンポーネントをインストール可能

## 事前準備（Agent用RHEL）
1. `concert_vm.yml`の作成

    `group_vars/concert_vm.template.yml` のファイルコピーを作成して `group_vars/concert_vm.yml` として保存。

1. 以下の変数を環境に合わせて編集

    ```yaml
    ssh_host: 0.0.0.0 # TechZoneに記載のPublic IP!
    ssh_private_key_file: ~/.ssh/your_key.pem
    ibm_entitlement_key: "REPLACE_ME_WITH_ENTITLEMENT_KEY"
    ```

1. （オプション）Secure Coderのインストール

    Secure Coderをインストールする場合は、`group_vars/concert_vm.yml`で以下を設定：
    
    ```yaml
    install_securecoder: true
    ```

1. （オプション）Workflowsのスキップ

    Workflowsをインストールしない場合は、`group_vars/concert_vm.yml`で以下を設定：
    
    ```yaml
    install_workflows: false
    ```

1. デプロイ
    ```bash
    ansible-playbook playbooks/setup.yml
    ```
    
    実行内容：
    - firewalldの停止・無効化
    - DNS設定（Google DNS 8.8.8.8追加）
    - 必要なパッケージのインストール
    - Concert v3.0.0のダウンロードと展開
    - manage-prereqs-vmの実行（K3s、Helmのインストール）
    - registries.yamlの検証と修正（必要に応じて）
    - Concertセットアップの実行（最大90分）
    
    Concert セットアップ（`bin/setup`）は長時間かかるため、`async`/`poll=180`（3分間隔）で進行監視しています。
    最後に UI アクセス用 URL (`https://<ssh_host>`) を表示し、最終的に 4xx/5xx が出ないことを確認します。

1. アクセス

    デプロイ完了後、以下のURLでアクセス：
    ```
    https://<ssh_host>/
    ```
    
    **注意**:
    - TechZoneではポート12443は開いていないため、ポート443でアクセスします
    - 初回アクセス時はWorkflowsにリダイレクトされます
    - Resilience（旧Enterprise view）またはProtect（旧Developer focus）タブからConcertダッシュボードにアクセスできます

### Secrets の扱い
このリポジトリには秘密情報を含めないでください。`group_vars/concert_vm.template.yml` をコピーし、実値は `group_vars/concert_vm.yml` に記載してください（`.gitignore` 済み）。
どうしても別ファイルで管理したい場合のみ、以下のように上書き指定できます。
```bash
ansible-playbook playbooks/setup.yml -e @group_vars/concert_vm.yml
```
さらに安全にするには `ansible-vault` で暗号化してください。
```bash
ansible-vault create group_vars/concert_vm.yml
ansible-playbook playbooks/setup.yml -e @group_vars/concert_vm.yml --ask-vault-pass
```
