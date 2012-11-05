# OpenStack勉強会 9回 事前準備

## 勉強会にあたっての事前作業

勉強会に参加するにあたって、以下の作業を事前にしておいてください。

OpenStackのセットアップには多量のパッケージをインターネットからダウンロードするため、会場での設定作業は時間がかかってしまうためです。

### 必要なモノ

* ノートPC(仮想化ソフトが使用可能なもの)
* 仮想環境
    * KVM
    * VirtualBox
    * VMware など
* インターネットアクセス環境

### 仮想マシンの設定

仮想マシンの設定は以下のように設定します。

* vCPU: 1個以上
* Mem:  2GB以上
* Disk: 30GB以上
* NIC:  1個
* OS:   Ubuntu Server 12.04 LTS http://www.ubuntu.com/download/server

### Ubuntuのインストール

