### やろうとしていることを言語化してみる


### 思考記録
計算量だけ意識すると、step1と同じく両方から見てできそう。
0を見つけて、そこから両端を操作することも考えたけど0が複数あると複雑になりそうだな。
やはりstep1と同じ実装をとりあえずきれいに書いてみる。

right, leftでやっていたけど、配列ならstart, endのほうが自然かもしれない。

loopが終わる条件を意識したい。
ぱっと思いつくのは2つ。
- loop開始時点でstart, endがおなじインデックスを指している
- loop終了時点でstart, endが隣り合わせ、間にインデックスがない
loop中に平方数を作ってインデックスを進めたいんだけど、進めるインデックスがどちらかは両側の数字を比較するloop回と同じじゃないといけないので、インデックスを進めるのは毎回やる必要がある。
そう考えるとstep1と違って、終了時点でstart, endが隣り合わせなことを確認したい気持ちになってきたな。
end - start == 1みたいな条件式になってぱっと見はわかりづらいけど、いったんこれで書いてみる。(まさかのrubyはendが予約語だった)
ここまで15min程度。

あと、出力用の配列に挿入するときはstep1のようにわざわざカウンタを用意しなくてもいけそうなので、ちょっと考える。
というか先頭に入れればいいだけだな。
そう考えるとLinkedListを使えばin-placeというか、追加のメモリを使わずに実装できる気もしてきた。
rubyにはLinkedListないらしい。
→これはstep3で試そうとしたけど、rubyだとそもそも実装が難しいし、LinkedListだと任意の要素を取得するのにO(N)かかることに気づいた。

これはだめなんだ。
隣り合わせ状態で抜けてしまうと、最後の隣合せの要素が平方数の配列に入れられずに終わってしまう。
入れることもできるけど、あんまりきれいじゃないな。
> - loop終了時点でstart, endが隣り合わせ、間にインデックスがない

やはりstep1と同様に2ポインタが同じところを指したら、最後にその要素を追加して終了が分かりやすそう。

### 計算量
#### 時間計算量

#### 空間計算量


### コード
```ruby
# @param [Array<Integer>] nums
# @return [Array<Integer>]
def sorted_squares(nums)
  sorted_squares = []
  head = 0
  tail = nums.length - 1
  nums.each do |n|
    return sorted_squares.prepend(nums[head] ** 2) if head == tail
    
    if nums[head].abs >= nums[tail].abs
      sorted_squares.prepend(nums[head] ** 2)
      head += 1
    else
      sorted_squares.prepend(nums[tail] ** 2)
      tail -= 1
    end
  end
end
```
### かかった時間


### 感想
- 