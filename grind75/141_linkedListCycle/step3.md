### やろうとしていることを言語化してみる


### 思考記録
問題をみてすぐにコードがイメージできるようになった。
rubyの構文はまだなれないので細かいところはシュッとでてこない。

入力上限が1万なので、1万個目のnextまで確認する解法とtwo pointerの解法も書いてみる。

### 計算量
#### 時間計算量

#### 空間計算量


### コード
```ruby
# ベーシック
def hasCycle(head)
    visited_nodes = Set.new
    cursor = head
    while !cursor.nil?
        visited_nodes << cursor
        return true if visited_nodes.include?(cursor.next)
        cursor = cursor.next
    end
    false
end
```


```ruby
# 鳩の巣原理
def hasCycle(head)
    cursor = head
    1.upto(10_001) do |_n|
        return false if cursor.nil?
        cursor = cursor.next
    end
    true
end
```

```ruby
# two pointer
def hasCycle(head)
    return false if head.nil? || head.next.nil?

    slow = head
    fast = head

    while !fast.nil?
        slow = slow&.next
        fast = fast&.next&.next
        return true if slow.equal?(fast)
    end
    false
endue
end
```
### かかった時間
ベーシックは2min
鳩の巣原理は5min
two pointerは20min


### 感想
- 鳩の巣原理、本当にうまくいった。面白い。
- two pointerは最初にガード説が必要なことに初見で気づけなかった
  - たしかにwhileで1歩進んだnodeと2歩進んだnodeを比べる前提条件に、それぞれ1歩先、2歩先に進めることがあるから最初に確認しないとだめ
  - 最初に1歩、2歩進めることを保証した後はその先はnilであればすぐに抜ける仕様になっているので、毎回1,2歩先に進めることを確認する必要はない
- 一目でやっていることや計算量がわかりやすいのはベーシックの実装なので、自分だったらこれを選ぶ
  - 鳩の巣原理は入力上限がはっきりわかっているときしか使えないので、実際に運用しているアプリケーションでは入力レンジが変わることはよくありそうだから使いづらいかなぁ
  - two pointerはガード句がwhileでやりたいことを保証するための前提条件になっているけど、それが伝わりづらいと自分は感じるから選ばないかも
  - メモリ要件がシビアなら、メモリ要件が厳しいからという理由のコメントを書いてtwo pointerを採用しそう