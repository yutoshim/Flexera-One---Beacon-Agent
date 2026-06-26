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

## インストール（Agent用RHEL）
