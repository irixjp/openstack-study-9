# OpenStack勉強会 第9回 プログラム

### プログラム
|項番|項目|講師(敬称略)|時間|
|---|---|---|----|
|0|事前作業|−|当日まで|
|1|devstackフォロー|中島|30分|
|2|基本的な使い方|中島|60分|
||休憩|−|15分|
|3|各コンポーネントの解説|−|時間切れまで|
|3-1|OpenStackコンポーネントの基本構造||15分|
|3-2|keystone||30分|
|3-3|glance||30分|
|3-4|cinder||30分|
||休憩|−|15分|
|3-5|quantum||60分|
|3-6|nova||60分|

### サポート係
|名前(敬称略)|備考|
|----|----|
|齊藤||
|石川||

## 当時までにやっておく作業
* devstackを使った環境構築
* https://github.com/irixjp/openstack-study-9/blob/master/README.md


## devstackフォローアップ
* 上手く設定できなかった向けのフォロー


## 基本的な使い方

https://github.com/irixjp/openstack-study-9/blob/master/1-basic-op.md

1. horizon経由
    1. ログイン
    2. ネットワークの作成
    3. 仮想マシンの作成
    4. ボリュームの作成&アタッチ
2. クライアントコマンド
    1. 利用のための設定
    2. horizonと同一


## 各コンポーネントの解説

解説の流れ

1. アーキテクチャ解説
2. OpenStackのコマンド or Dashboard で操作
3. Linux側での現象を確認

という流れですすめます。


### OpenStackコンポーネントの基本構造
1. APIサーバとクライアントライブラリ


### keystone
1. Keystoneのアーキテクチャ
   1. テナント、ユーザ、ロール
   2. 認証、トークン、エンドポイント
2. curlで直接APIを叩いてレスポンスを確認


### glance
1. Glanceのアーキテクチャ
    1. glance-api
    2. glance-registory
2. イメージの確認
3. スナップショットの確認


### cinder
1. cinderのアーキテクチャ
2. ボリュームの作成
3. アタッチ


### quantum
1. quantumのアーキテクチャ
    1. quantum-server
    2. quantum-agent
    3. quantum-dhcp
    4. quantum-l3
2. ネットワークの作成
3. サブネットの作成
4. ルーターの作成
5. 複数セグメントの連結

### nova
1. novaのアーキテクチャ
    1. nova-api
    2. nova-scheduler
    3. nova-cert,console,consoleauth
    4. nova-compute
2. キューとDB
3. 仮想マシン作成の流れ
