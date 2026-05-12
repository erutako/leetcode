### やろうとしていることを言語化してみる


### 思考記録
O(N)でいけそうな感覚。
小さい数字から順に重複があればnextから外して、数字が変わるときにその数字をnextに入れるをやっていけば良さそう。
昇順ソートが保証されていて、Node数の上限も300だからCycleはない。
Node.valは負の数もある。
まずは小さい数から順に見てやれるかコード書いてみる。
要素数0はコーナーケースとして注意。

一度コードを書いたらCase 2でWrong Answer。
そうか、最後はnilに到達したときにそれまで同じ値が連続しているとLinkedListの連結が残ったままだ。
処理中の値の先頭のLinkedListのnextは毎回nilで初期化して問題なさそうなのでそうする。

[1,1]でWrong Answer。
あー、そうか、nextをnilにするのは最初にしとかないとだめだ。

### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(1)

### コード
```ruby
def delete_duplicates(head)
    return head if head.nil?

    deleting_dup_num_head = head
    cursor = head.next
    deleting_dup_num_head.next = nil

    while !cursor.nil?
        if cursor.val == deleting_dup_num_head.val
            cursor = cursor.next
            next
        end

        deleting_dup_num_head.next = cursor
        deleting_dup_num_head = cursor
        cursor = cursor.next
        deleting_dup_num_head.next = nil
    end

    return head
end
```
### かかった時間
30min

### 感想
- ループ後に重複削除が終わっている状態にしたいときの各ループでの前提条件がちゃんとわかっていなかったので、テストケースで複数落ちてしまった
- `deleting_dup_num_head`はちょっと長いかも
  - headはなくても良さそうだけど、あると自分がイメージしている処理は表せている
  - でも、各値でheadを保持してそこから重複排除するという書き方自体がちょっとコードを読みづらくしている気がする
  - 今回は変数名つけるの難しいな
    - https://github.com/Satorien/LeetCode/pull/2#discussion_r2032238007
- rubyだと`while !cursor.nil?`は`while cursor`が自然らしい
  - 違いは`while cursor`はcursorがfalseでもループを抜けること
  - 今回はcursorはListNodeであることが明らかなので、素直に読める`while cursor`が良さそう
- テストケースに失敗してから気づいたけど、切ってから繋ぐということを自分は前提条件で野郎としていた
  - これは現実世界でやろうとしたらどうなるかを想像すると自然と出てくる発想な気がする
  - ここを意識できていなかったので、場当たり的にnextにnilを入れるコードになってしまった
  - 更に言うと、切ってから繋ぐ以外にも隣接が同じ値であればそこは入れ替え可能なのでわざわざunlinkしなくても次のListNodeで置き換えるだけでよかった
  - 前提条件を正確に捉えることが難しい
    - https://github.com/fv17/coding-practice/pull/3#discussion_r2595299326
- deleting_dup_num_headは「その値ブロックの先頭」というより、実際には 「重複除去後のリストの末尾ノード」なんだな
  - これが意識できていないのでコードがわかりづらくなってしまった
  - 無意識に数字が連続して重複しているブロックがいくつかあって、各ブロックを処理して繋いでいくという前提からコードを書き始めてしまったが、[1,1]みたいにブロックが1つしかないパターンを想像できていなかった
  - 既存のLinkedListから各数字ごとに重複を排除する発想でコードを書いたけど、最初から順に重複を省いてLinkedListのつなぎ方を変えていくという発想のほうが自然だった
- sortされていない値が入ってきたときどう処理するかは問題の制約とは別に考えても良さそう
