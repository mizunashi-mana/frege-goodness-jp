# 第一回 : 無限に怠惰に後回し

ボードゲームはお互いの手番の繰り返しで成り立っており、それぞれの手番によってボードの状態が更新されます。可能な着手は多数存在し、かつそのそれぞれに対して対戦相手による多数の返し手が存在するため、考えうるゲームの展開はボードの状態による木構造をなします。

Caption: 着手と返し手による木構造

```
start
  move option 1
    counter-move 1
    counter-move 2
  move option 2
    counter-move 3
    counter-move 4
```

ゲームのルールにもよりますが、このようなゲーム木は無限に大きくなりえます。

これを Frege や Haskell のような遅延評価を行う関数型言語でモデリングする自然な方法は、木のサイズについてはまったく気にせず、どうやって木を構築するかにのみ注意を払うことです。ここでは木を生成することに注目し、生成される木を制限する作業は後回しにします。

以下に挙げたのは、任意の型「a」の値を要素とする木を構築する関数です。要素である「a」型の値をすべて `unfold` することで、再帰的に各ノードの子要素となるノードを生成します。

Caption: 再帰による一般的な木の構成

```
buildTree :: (a -> [a]) -> a -> Tree a
buildTree unfold a = Node a children where
    children = map (buildTree unfold) (unfold a)
```