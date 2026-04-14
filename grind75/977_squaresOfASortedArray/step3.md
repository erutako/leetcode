### やろうとしていることを言語化してみる


### 思考記録
とりあえず問題をぱっと見てすぐ解けるか試す。
in-placeでやるのは難しい。
ワンパスではやれる。
descでsortされているので、squareだと両端が大きい数字になっている。
両端の平方数を比べながら、大きいものから順に配列に詰めていき、中心に向かって走査する。
最後にポインタが重なり合ったら、その要素を配列に入れて完成。

極端な入力で想定したほうが良さそうなのは
- 空配列 (問題の制約上は不要)
- 0のみ配列
- 左端が0の配列
- 正、負どちらかしかない配列

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

  nums.each do |_n|
    head_square = nums[head] ** 2
    tail_square = nums[tail] ** 2

    return sorted_squares.prepend(head_square) if head == tail

    if head_square >= tail_square
      sorted_squares.prepend(head_square)
      head += 1
    else
      sorted_squares.prepend(tail_square)
      tail -= 1
    end
  end

  # 空配列を想定
  return sorted_squares
end
```
### かかった時間
思考メモも含めて13min

### 感想
- Arai60の問題ではないので、他の方の議論を参考にできなかった
- rubyにLinkedListがないのは割と衝撃だった(良し悪しではなく)
  - LinkedListを作って解いてみた