### やろうとしていることを言語化してみる


### 思考記録
色々読んでみて、他の解答をパッとコードで表現できるかやってみる。
まずは非0を集めて、あとから0埋めするやつ。

### 計算量
#### 時間計算量

#### 空間計算量


### コード
#### スワップしていくやつ
```ruby
# @param [Array<Integer>] nums
# @return [void] Do not return anything, modify nums in-place instead
def move_zeroes(nums)
    write = 0
    nums.each_index do |read|
        next if nums[read].zero?
        nums[write], nums[read] = nums[read], nums[write]
        write += 1
    end
end
```

#### 非0をまとめてから0埋めする
``` ruby
# @param [Array<Integer>] nums
# @return [void]
def move_zeroes(nums)
  non_zero_length = 0;
  nums.each.with_index do |n|
    unless n.zero?
      nums[non_zero_length] = n
      non_zero_length += 1
    end
  end
  nums.fill(0, non_zero_length, nums.length - non_zero_length)
end

### かかった時間
2min

### 感想
- [「いくつ非ゼロが見つかったか」というように見ています。](https://github.com/rihib/leetcode/pull/50/changes#r1889742988)
  - なるほどなぁ。非ゼロを見つけた数だけ左から置いていっていると考えると美しい。インデックスが0始まりなのとうまく噛み合って、なんかきれいになる。
- [そうすると、あとから0を埋めるというのも一つです。](https://github.com/rihib/leetcode/pull/50/changes#r1890202244)
  - これも確かに。やりたいことの最終形が見えていればとりあえず全部左に寄せたらあとは0埋めでいいやんという発想になる。
  - 自分が書いてて、writeとreadをスワップすることの違和感がwriteに書き込むことに意味はあるけど、writeにあったものをreadにすることはそんな意味がないことで、それがこの考え方だとスッキリする。
    - とりあえず全部左に寄せたいのがやりたいこと
    - そう考えると名前の付け方も変わりそう
      - non_zero_lengthとかかなぁ
- [fhiyo-sanのコメント](https://github.com/rihib/leetcode/pull/50/changes#r1888174862)は今回の問題の本質だなぁ
  - > 個人的にこの問題はループ不変条件が何かを意識するのが大事な気がしており、今回であればzeroIndexより左と、zeroIndexからiまでが各ループの終わりで何が入っているか書いてあると分かりやすいかなと思います
- [fhiyo-san解答](https://github.com/fhiyo/leetcode/pull/54/changes#diff-2f8b85074aa38861aa9dd6fbe0c5f1b540a06f8618d7552b4ffd05da21f795d3R7-R8)
  - 全然関係ないけどfhiyo-sanも初見で14min位かかっているのか(十分すごいけど)と思うと、こういうのは今までの経験値というかこういうトレーニングを積んで、知識や理屈を頭の中に入れられているかどうかによるところが大きいなという気持ち。
  - 3パスの回答はめっちゃきれい。やっぱりこの問題を見たときにイメージするやりたいことは多分これ。
    - スワップが云々というよりは、0を右に動かすという問題の見せ方に対して同じサイズの配列でもとの配列の非0が左から順にうまってたらええんやろという気持ち。
      - これをワンパスで表現すると解答例になると思ったがちょっと違うな。スワップせずに無視して左寄せして最後に埋めすればいいからちょっと違う
      - 最後に0埋めするやりかたはin-placeじゃできないと思っていただけど、fhiyo-sanのstep2nd見てたらできるな。
- rubyでfill使うとC言語レベルで最適化されるらしい
  - これは時間あったらソース読みたい