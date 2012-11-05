# OpenStack勉強会 第9回 プログラム

## 当時までのやっておく作業
* devstackを使った環境構築
* https://github.com/irixjp/openstack-study-9/blob/master/README.md


## devstackフォローアップ
* 上手く設定できなかった向けのフォロー


## 基本的な使い方
1. horizon経由
    1. ログイン
    2. ネットワークの作成
    3. 仮想マシンの作成
    4. ボリュームの作成
2. クライアントコマンド
    1. 利用のための設定
    2. horizon経由と同一


## 各コンポーネントの解説
1. keystone
2. glance
3. nova
    1. nova-api
    2. nova-scheduler
    3. nova-cert,console,consoleauth
    4. nova-compute
4. cinder
5. quantum
    1. quantum-server
    2. quantum-agent
    3. quantum-dhcp
    4. quantum-l3
