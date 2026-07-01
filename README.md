# Flexera-One---Beacon-Agent

FlexeraOne ITAM利用ガイド_v2.0.pptx をベースに TechZone に Beacon と Agent をインストールして接続してみる
https://ibm.ent.box.com/file/2175013902181

## 前提条件
- https://ibm.app.flexera.com/orgs/33975 にログインできること
- TechZone にアクセスできること


## 事前準備（Beacon用Windows）
- TechZone で Windows Server 2022 IBM Cloud VPC を予約する 
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
- 上記完了後 PORT 80/443 を開けるためのサポートチケットをあげる（下記のような文面を入力すればやってくれる）https://ibmsf.my.site.com/ibminternalproducts/s/createrecord/NewCase
    ```yaml
    Please make port 80/443 accessible from the external.
     - https://techzone.ibm.com/my/requests/XXXXXXXXXXXXXXX
     - Name: XXXXXXXXXXXXXXX
     - Request ID: XXXXXXXXXXXXXXX
    ```

## 事前準備（Agent用RHEL）
- TechZone で RHEL 9 IBM Cloud VPC を予約する 
  - Geography は itz-vpc-02 - ap - jp-tok region - jp-tok-3 datacenter を選択する（多分他でもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 ssh_private_key.pem をダウンロードして、Mac のターミナルから VM にアクセスできることを確認


## インストール（Beacon）
- Windowsにログイン 
- Windows Remote Management Serviceの有効化
  - Windows PowerShell起動（左下メニューから）
    ```yaml
    runas /user:Administrator powershell　# Administratorパスワードの入力
    Enable-PSRemoting -Force
    ```
- IISインストール（参考 https://qiita.com/carol0226/items/4357e773efdbb08b5c52 ）
  - Server Manager起動
    ```yaml
    runas /user:Administrator powershell
    Start-Process ServerManager.exe runAs
    ```
  - IIS設定
    - 基本はデフォルトでOKだが、下記は注意
    -  ![alt text](./images/IIS.png)
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認

- ファイアウォールの無効化（ほんとは PORT 80/443 だけでいいかも）
  - Server Manager起動
    ```yaml
    runas /user:Administrator powershell
    Set-NetFirewallProfile -Profile Domain,Private,Public -Enabled False
    ```

- Flexera Oneにログイン（ https://ibm.app.flexera.com/orgs/33975 ）


- Beacon ダウンロード（ FlexeraOne ITAM利用ガイド_v2.0.pptx の P45 ）


- Beacon Installer起動
    ```yaml
    runas /user:Administrator powershell
    Start-Process C:\Users\itzuser\Downloads\BeaconInstaller25.5.0.9
    ```

- Beacon インストール（ FlexeraOne ITAM利用ガイド_v2.0.pptx の P46,47 ）
  - Server Manager起動
    ```yaml
    runas /user:Administrator powershell
    Start-Process ServerManager.exe runAs
    ```

- Beacon 起動
    ```yaml
    runas /user:Administrator powershell
    Start-Process "C:\Program Files\Flexera Software\Inventory Beacon\DotNet\bin\InventoryBeacon.exe"
    ```

- ビーコンの構成（ FlexeraOne ITAM利用ガイド_v2.0.pptx の P49,50 ）
  - メニュー「Data Collection/IT Assets インベントリ タスク/ビーコン」
 

  
## インストール（Agent用RHEL）
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
- Windows Remote Management Serviceの有効化
  - Geography は itz-vpc-01 - americas - us-east region - us-east-3 datacenter を選択する（多分どっちでもOK）
  - Configuration で Customizeして、Enables a public internet facing ip. を On にし、Security hardening は Off にする）
  - VM が Ready になったら、 request ページを開いて Guacamole remote desktop にアクセスできることを確認
