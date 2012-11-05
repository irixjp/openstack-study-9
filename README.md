# OpenStack勉強会 9回 事前準備

勉強会に参加するにあたって、以下の作業を事前に行なってください。

OpenStackのセットアップには多量のパッケージをインターネットからダウンロードするため、会場での設定作業は時間がかかってしまいます。

## 必要なモノ

* ノートPC(仮想化ソフトが使用可能なもの)
* 仮想環境
    * KVM
    * VirtualBox など
* インターネットアクセス環境

## 仮想マシンの設定

仮想マシンの設定は以下のように設定します。

* vCPU: 1個以上
* Mem:  2GB以上
* Disk: 30GB以上
* NIC:  1個
    * ブリッジネットワークは「使わない」
    * 物理ホスト上の内部ネットワークとして構成してください。
* OS:   Ubuntu Server 12.04 LTS http://www.ubuntu.com/download/server

## Ubuntuのインストール

以下の設定でインストールを行います。

* (1) インストーラー言語の選択
    * “English” を選択
* (2) インストールディスクの起動メニュー
    * “Install Ubuntu Server” を選択
* (3) 言語の選択
    * “English” を選択
* (4) 地域の選択
    * “Other” → “Asia” → “Japan” を選択
* (5) ロケールの選択
    * “United States” を選択
* (6) キーボードの自動検出
    * ”No” を選択
* (7) キーボードの選択
    * 自分のキーマップにあったものを選択
* (8) ホスト名 を入力
    * ubuntu
* (9) 一般ユーザを作成
    * username: openstack
    * password: openstack
* (10) タイムゾーンの選択
    * “Asia/Tokyo” を選択
* (11) パーティションの作成
    * “Guided – use entire disk and set up LVM” を選択
* (12) パーティションの確認
    * / 領域に20GB以上割り当てられている事を確認
* (13) HTTP Proxyの選択
    * 必要であれば設定
* (14) Taskselの設定
    * “No automatic updates” を選択
* (15) ソフトウェアの選択
    * “OpenSSH Server” のみ選択
* (16) Grubのブートローダーをインストール
    * “Yes” を選択
* (17) インストールの終了
    * “Continue” を選択して再起動

こちらも参考にしてください。

http://www.ospn.jp/press/20120828no27-useit-oss.html

## インストール後の基本設定

openstackユーザでログイン後、以下の設定を実行してください。

### アドレスの固定化

ここで固定化するアドレスは以下の要件を満たす様に設定してください。
* インターネットへアクセス可能
* ホストマシンからssh/httpでのアクセスが可能

以下は設定の一例です。

    $ sudo vi /etc/network/interfaces
    ------------------------
    auto eth0
    iface eth0 inet static
    address 192.168.128.100
    netmask 255.255.255.0
    gateway 192.168.128.1
    dns-nameservers 192.168.128.1
    ------------------------

### git のインストール

    $ sudo apt-get update
    $ sudo apt-get install -qqy git

## openstackの設定

openstackの設定には、devstackという開発者向けの簡易設定ツールを利用します。

### devstackのリポジトリ取得

    $ cd ~
    $ git clone https://github.com/openstack-dev/devstack.git
    $ cd ~/devstack

    $ git checkout -b folsom remotes/origin/stable/folsom

### devstackの設定

~/devstack/localrc ファイルを作成して、devstackの設定を行います。

    $ vi ~/devstack/localrc
    ------------------------
    HOST_IP=192.168.128.100

    ADMIN_PASSWORD=openstack
    MYSQL_PASSWORD=$ADMIN_PASSWORD
    RABBIT_PASSWORD=$ADMIN_PASSWORD
    SERVICE_PASSWORD=$ADMIN_PASSWORD
    SERVICE_TOKEN=admintoken

    disable_service n-net
    disable_service n-obj
    enable_service q-svc
    enable_service q-agt
    enable_service q-dhcp
    enable_service q-l3

    ENABLE_TENANT_TUNNELS=True

    FIXED_RANGE=172.24.17.0/24
    NETWORK_GATEWAY=172.24.17.254
    FLOATING_RANGE=10.0.0.0/24

    NOVA_BRANCH=stable/folsom
    GLANCE_BRANCH=stable/folsom
    KEYSTONE_BRANCH=stable/folsom
    HORIZON_BRANCH=stable/folsom
    CINDER_BRANCH=stable/folsom
    QUANTUM_BRANCH=stable/folsom
    ------------------------

* HOST_IP
    * 固定化したIPアドレスを指定します。


### devstackの実行

    $ cd ~/devstack
    $ ./stack.sh

正常に終了すると以下のメッセージが表示されます。

    Horizon is now available at http://192.168.128.100/
    Keystone is serving at http://192.168.128.100:5000/v2.0/
    Examples on using novaclient command line is in exercise.sh
    The default users are: admin and demo
    The password: openstack
    This is your host ip: 192.168.128.100
    stack.sh completed in 1104 seconds.

### 動作確認

ブラウザから仮想マシンのアドレスへアクセスし、OpenStackのログイン画面が表示されるか確認してください。

### OpenStack停止時の注意

OpenStack上で仮想マシンを作成した場合、OpenStack停止(Ubuntuの停止)前に仮想マシンを削除しておいてください。

### Ubuntu再起動後の作業

devstackにはいくつか揮発性の設定が含まれているため、Ubuntuを再起動した場合、以下の設定を手動で行う必要があります。

    $ sudo ip addr add 10.0.0.1/24 dev br-ex
    $ sudo ip link set br-ex up
    $ sudo route add -net 172.24.17.0/24 gw 10.0.0.2
    $ sudo losetup -f --show /opt/stack/data/stack-volumes-backing-file

Ubuntu再起動後にdevstackで作った環境を起動するには以下のコマンド実行します。

    $ cd ~/devstack
    $ ./rejoin-stack.sh

