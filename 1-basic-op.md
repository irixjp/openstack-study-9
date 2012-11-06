# 基本操作

ここではダッシュボード、コマンドラインを使ってOpenStackを利用してみます。


## ダッシュボードからのアクセス

ブラウザからOpenStackのダッシュボードのアドレスへアクセスします。


### ログイン

demo/openstack でログインします。


### テナントの選択

demoテナントを選択します。


### キーペアの作成

これから作成する仮想マシンへのログインのため、SSHキーペアを作成しておきます。

* キーペアの作成
    * キーペア名
        - testkey01

ダウンロードされた testkey.pem をUbuntuへコピーしておきます。

    physic$ scp testkey01.pem openstack@(ubuntuサーバ):~

    ubuntu$ chmod 600 testkey01.pem


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

* Access & Security
    - keypair
        - testkey01
    - Security Groups
        - default レ

* Networking
    - net1 レ


これで仮想マシンが作成されます。

    ubuntu$ sudo virsh list

    Id Name                 State
    ----------------------------------
    1  instance-00000001    running


#### ログの確認

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


#### コンソールへの接続

noVNCを使って仮想マシンのコンソールへアクセスできます。


### セキュリティの設定

このままでは作成した仮想マシンにアクセスできませんので、セキュリティを変更します。

    ubuntu$ ping 仮想マシンのアドレス
    ubuntu$ ssh -i ~/testkey01.pem cirros@仮想マシンのアドレス


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

この設定することで仮想マシンへのアクセスが可能になります。

    ubuntu$ ping 仮想マシンのアドレス
    ubuntu$ ssh -i ~/testkey01.pem cirros@仮想マシンのアドレス


### ネットワークの作成

* ネットワークの作成
    * Network
        - net2
    * Subnet
        - Create Subnet レ
        - Network Address
            - 10.10.10.0/24
        - IP Version
            - IPv4
        - Gateway IP
            - 10.10.10.254

このネットワークに仮想マシンを接続するように、新しいインスタンスを作成します。

* Details
    - Instance Source
        - Image
    - Image
        - cirros-0.3.0-x86_64-uec
    - Instance Name
        - testvm02
    - Flavor
        - m1.tiny
    - Instance Count
        - 1

* Access & Security
    - keypair
        - testkey01
    - Security Groups
        - default レ

* Networking
    - net2にチェック

先に作った仮想マシンと、新しい仮想マシンがアイソレーションされている事を確認します。

    ubuntu$ ssh -i ~/testkey01.pem cirros@(testvm01のアドレス)

    testvm01$ ping (testvm02のアドレス)


### ボリュームの作成&アタッチ

ボリュームを作成して仮想マシンへアタッチしてみます。

* ボリューム作成
    * ボリューム名
        - testvol01
    * 容量(GB)
        - 1GB

このボリュームを仮想マシン testvm01 へアタッチしてみます。

接続前の仮想マシン側のディスク状態

    ubuntu$ ssh -i ~/testkey01.pem cirros@(testvm01のアドレス)

    testvm01$ sudo fdisk -l

    Disk /dev/vda: 25 MB, 25165824 bytes
    16 heads, 63 sectors/track, 48 cylinders, total 49152 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x00000000

    Disk /dev/vda doesn't contain a valid partition table


ボリュームをアタッチします。

* ボリュームの接続の管理
    * インスタンスへの接続
        - testvm01
    * デバイス名
        - /dev/vdb

再度、仮想マシン側の状態を確認します。

    ubuntu$ ssh -i ~/testkey01.pem cirros@(testvm01のアドレス)

    testvm01$ sudo fdisk -l

    Disk /dev/vda: 25 MB, 25165824 bytes
    16 heads, 63 sectors/track, 48 cylinders, total 49152 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x00000000

    Disk /dev/vda doesn't contain a valid partition table

    Disk /dev/vdb: 1073 MB, 1073741824 bytes
    16 heads, 63 sectors/track, 2080 cylinders, total 2097152 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x00000000

    Disk /dev/vdb doesn't contain a valid partition table

仮想マシンに新しいデバイスが接続されていることが確認できます。


## コマンドライン操作

### 利用のための設定

openstackコマンドラインを使うためにはいくつかの環境変数を設定する必要があります。

    ubuntu$ cd ~/devstack
    ubuntu$ source openrc
    ubuntu$ env |grep OS_ |sort

    OS_AUTH_URL=http://157.7.133.23:5000/v2.0
    OS_NO_CACHE=1
    OS_PASSWORD=^openstack2012$
    OS_TENANT_NAME=demo
    OS_USERNAME=demo


### コマンドラインからの操作

    $ nova list



