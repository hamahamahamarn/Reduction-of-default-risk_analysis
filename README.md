# Reduction-of-default-risk_analysis
# データについて
[Signateの債務不履行リスクの低減コンペティションページ](https://signate.jp/competitions/983)    
データ数：242150、特徴量の数：10
|変数名|詳細|  
|:---|:---|  
|id	|顧客ID  |
loan_amnt	|借入総額  
term	|返済期間  
interest_rate	|金利  
grade	|グレード  
employment_length	|勤続年数  
purpose	|借入の目的  
credit_score	|信用スコア
application_type  |借入時の申請方式
`loan_status`  |返済状況 `目的変数` 

# EDA、特徴量エンジニアリングについて
- 目的変数について不均衡データであることが判明した。
- 箱ひげ図より外れ値の存在を確認。
- ヒストグラムより、`loan_amnt`, `interest_rate`, `credit_score`について多峰性が見られるため、それぞれの峰ごとに分けた特徴量を作成すると良いと予想した。
- 目的変数の要素を数値に変換
- `term`, `employment_length`は[3 years]のように本来数値であるものが文字として認識されてしまっているので数値に直す。
- `purpose`に含まれる要素[moving]をotherにまとめて処理する
- interest_rateとcredit_scoreについて多重共線性の疑いがもたれる。
- `grade`と`application_type`について順番が関係すると考え、ラベルエンコーディングで処理した。
- それ以外のカテゴリカル変数はダミー化(OneHotEncoding)により処理した。
- 多重共線性を考慮して特徴量を削除すると精度が悪化した。  
ある程度はlightgbmのパラメータ(正則化項)で対処できていると予想している。

# モデルを用いた分析
## バリデーション（hold-out法、k分割交差検証）
- データを8:2で分割した。
- 不均衡データであることを考えると目的変数で層化k分割交差検証でもよかったかもしれない。
## 使用した機械学習モデル
- 一番精度の良かったlightgbmを使用。
- optunaによるハイパーパラメータのチューニングを行う。
- 閾値調整をして一番f1_scoreが高い閾値を探す。->0.20が最適と判断

# 振り返り
- もう少し分析日数があればさらに精度向上が出来た気がする。
- 特徴量エンジニアリングの部分で他にも目的変数に関係する特徴量を作成できた可能性がある。
- 多峰性の見られた特徴量について数値の分ける基準を明確にする必要があると感じた。（今回はグラフから読み取って設定した。）
- 層化k分割交差検証についても詳しく検証してみたかった。
- アンサンブルなどを試してさらに精度向上が出来るかどうか確認したい。
- データ分析コンペティションでは、精度向上が一番だが、実際に施策を提案するとなった場合は、説明しやすいモデルを使用することや特徴量重要度などを算出したりするなど、他者が分かりやすいように説明する必要があると感じた。
