# 概要

英語の音節数を計算するツールです。  
nltkのcmudict(カーネギーメロン大学が作成した発音辞書)をベースとして、非収録語にもある程度対応できるアルゴリズムを採用しています。  

# セットアップ
```
pip install count-syllable
```

# アンインストール
```
pip uninstall count-syllable nltk
```

# 使用方法
```
from count_syllable import count_syllable

data = count_syllable("anyone")

print(data)
```

# アルゴリズム（前方後方一致法）

前方単語ブロック、後方単語ブロック、中間単語ブロックに分けて考える。  

(1)単語がcmudictに存在するか参照しあればその音節数を前方単語ブロックの音節数とする、なければ後方文字を一文字削減して再度参照し、2文字以下になるまで繰り返す  
(2)2文字以下になった場合は、母音の数を計算し、前方単語ブロックの音節数とする  
(3)前方単語ブロックを除いた単語がcmudictに存在するか参照しあればその音節数を後方単語ブロックの音節数とする、なければ前方文字を一文字削減して再度参照し、2文字以下になるまで繰り返す  
(4)2文字以下になった場合は、母音の数を計算し、後方単語ブロックの音節数とする  
(5)前方単語ブロック、後方単語ブロックを除いた単語について、母音の数を計算し、中間単語ブロックの音節数とする  
(6)2文字以下になった単語をまとめて、二重母音の数を計算する  
(7)前方単語ブロック、後方単語ブロック、中間単語ブロックの音節数を加算し、二重母音の数を減算して、単語の音節数を求める  
  
How Many Syllablesを用いたデータセットとの比較評価実験の結果、cmudictベースのものは71.2＋α％の精度、HMS音節数データセットを用いると94.6＋α％の精度になることが分かっている。
ただし、 HMS音節数データセットは著作権の問題があるため、無償で利用するのは難しいと考える。    

# 略語辞書
  
enwiktionary（https://dumps.wikimedia.org/enwiktionary/）において、本文に「initialism of」を含むタイトルを収集し、文字数を音節数として設定している。  
専門用語までカバーしきれていないため、略語辞書に存在しないアルファベット大文字のみで構成される単語についても、文字数を音節数とする対応を検討中である。  
  
# 関連ツール、関連サイト

## Sylco: https://github.com/eaydin/sylco
独自アルゴリズムで英語の音節数を計算するツール。
ライセンスが非記載のため、利用用途は限られるが、アルゴリズムは参考になる。
  
## How Many Syllables: https://www.howmanysyllables.com
5つの手法を用いて英語の音節数を計算するサイト。  
詳細な使用アルゴリズムは不明だが、多くの英単語に対応している。  
  
# 論文

- 赤木信也：変数置き換えモデルを用いた英日両文に適用可能なリーダビリティ判定ツールの開発と公開
	- 情報処理学会研究報告,2025,NL-264,No.8,pp.1-10,2025-06-29
	- https://jglobal.jst.go.jp/detail?JGLOBAL_ID=202502284060321622
- 赤木信也：英語音節数データセットの作成に関する一考察
	- 情報処理学会第88回全国大会
	- https://www.ipsj.or.jp/event/taikai/88/
  
# ライセンス
- count-syllable
	- Python Software Foundation License
	- Copyright (C) 2024 Shinya Akagi
- nltk
	- Apache License 2.0
	- Copyright (C) 2001-2023 NLTK Project
- cmudict
	- BSD License
	- Copyright (C) 1998 Carnegie Mellon University
  
