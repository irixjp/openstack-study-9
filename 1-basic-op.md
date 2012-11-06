# 基本操作

ここではダッシュボード、コマンドラインを使ってOpenStackを利用してみます。


## ダッシュボードからのアクセス

ブラウザからOpenStackのダッシュボードのアドレスへアクセスします。


### ログイン

demo/openstack でログインします。


### テナントの選択

demoテナントを選択します。


### 仮想マシンの作成

仮想マシンを作ってみます。

* Details
    - Instance Source
        - Image
    - Image
        - cirros-0.3.0-x86_64-uec
    - Instance Name
        - testvm01
    - Flavor
        - m1.tiny
    - Instance Count
        - 1
* Networking
    - net1にチェック


これで仮想マシンが作成されます。

* ログの確認
ログに仮想マシンのユーザ、パスワードが表示されていますのでメモしておいてください。

    instance-id: i-00000001
    public-ipv4:
    local-ipv4 : 172.24.17.2

    wget: server returned error: HTTP/1.1 404 Not Found
    cloud-userdata: failed to read user data url: http://169.254.169.254/2009-04-04/user-data
    WARN: /etc/rc3.d/S99-cloud-userdata failed
       ____               ____  ____
      / __/ __ ____ ____ / __ \/ __/
     / /__ / // __// __// /_/ /\ \ 
     \___//_//_/  /_/   \____/___/ 
      http://launchpad.net/cirros

    login as 'cirros' user. default password: 'cubswin:)'. use 'sudo' for root.


* コンソールへの接続


### セキュリティの設定

このままでは作成した仮想マシンにアクセスできませんので、セキュリティを変更します。

    ubuntu$ ping 仮想マシンのアドレス
    ubuntu$ ssh cirros

* default ルールの編集
    * ssh
        - TCP
        - ポート番号(下限)
            - 22
        - ポート番号(上限)
            - 22
        - 元グループ
            - CIDR
        - CIDR
            - 0.0.0.0/0

    * ping
        - ICMP
        - ポート番号(下限)
            - -1
        - ポート番号(上限)
            - -1
        - 元グループ
            - CIDR
        - CIDR
            - 0.0.0.0/0


Ubuntuサーバから以下のコマンドを実行します。




### ネットワークの作成



### ボリュームの作成&アタッチ


## コマンドライン操作

### 利用のための設定

### 

