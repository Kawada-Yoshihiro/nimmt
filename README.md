# nimmt
ニムト関連プログラム

## ゲームの流れ
1) プレーヤー側のインスタンスを作る → リストにする。
2) プレーヤーインスタンスのリストを渡して、ディーラーインスタンスを作る (各プレーヤーに *dealer* のインスタンスを教える (各プレーヤーの `get_know_dealer()` が呼ばれる)。カードも配られる (各プレーヤーの `get_hand()`)。
3) ディーラーが各プレーヤーのカードを 1 枚ずつ得る (各プレーヤーインスタンスの、`put_card()` が呼ばれる)
4) ディーラーが受け取ったカードを場に並べていく。ただし、場のどのカードよりも小さい数のカードが出された場合は、そのプレーヤーインスタンスにどの列を引き取るか訊く (対応プレーヤーの、`taking_column()` が呼ばれる)。その列のカードがなんだったか記録される (*__earned_cards* に格納)。また、6 枚目のカードを出したプレーヤーにも前の 5 枚が記録される。どの場合も、即座に場のカードが更新される。
5) 手順 3,4 を繰り返す。
7) 最後にスコアを表示する。
8) 手順 2-7 を繰り返して、誰かの合計スコアが 66 を超えた時点で終わる。

## Player クラスに必須のメソッドは 5 つ
`get_know_dealer()` ディーラー側で呼んで、ディーラーのインスタンスにアクセス可能にする (変更不要)  
`get_hand()` ディーラー側で呼んで、手札を得る (変更不要)  
`put_card()` ディーラー側で呼んで、どのカードを出すか知らせる (変更可能: 基本クラスでは手札の左から順番に出すアホな仕様)  
`taking_column()` 一番小さい数を出したときに Dealer 側で呼んで、どの列を引き取るか知らせる (変更可能: 基本クラスでは一番上の列を引き取るアホな仕様)  

## ディーラーから得られる情報 = @property 属性のもの
`self.dealer.num_hand` 手札の初期枚数  
`self.dealer.num_field` 場のカード列の数  
`self.dealer.max_card` カードの最大値  
`self.dealer.num_max_column` 場の一列におけるカードの最大枚数  
`self.dealer.num_players` プレーヤー人数  
`self.dealer.field` 今のターンの場の状況  
`self.dealer.played_cards` 今のターンの各プレイヤーの出したカード  
`self.dealer.score` 現在のスコア  

## ゲームの実行
`python3 nimmt.py`

## 解析ツールの使い方
**必要条件**  
GNU の getopt が使えること  
### 複数回のゲーム実行
**実行方法**  
`sh play_nimmt.sh [オプション] [パラメーター]`  
`-h` オプションで、ヘルプが出るので参照。
### ゲーム結果の統計処理
**実行方法**  
`python3 format_score.py [オプション]`  
`-h` オプションで、ヘルプが出るので参照。