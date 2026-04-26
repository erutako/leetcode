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
  insert_pos = 0
  nums.each do |n|
    if !n.zero?
      nums[insert_pos] = n
      insert_pos += 1
    end
  end
  nums.fill(0, insert_pos..) if insert_pos < nums.length
  nums
end
```

#### Generatorで解く
```ruby
# @param {Integer[]} nums
# @return {Void} Do not return anything, modify nums in-place instead.
def move_zeroes(nums)
  non_zero_generator = Enumerator.new do |yielder|
    nums.each do |n|
      yielder << n if n != 0
    end
  end

  write_index = 0
  
  loop do
    nums[write_index] = non_zero_generator.next
    write_index += 1
  rescue StopIteration
    break
  end

  while write_index < nums.length
    nums[write_index] = 0
    write_index += 1
  end
end
```

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
      - これをワンパスで表現すると解答例になる。
      - でもin-placeでやるなら採用できないのか
        - 嘘、採用できる。そのまま配列を塗り替えていけばいい。
- Erase-Remove Idiom, loop unrollingという観点
  - https://github.com/fhiyo/leetcode/pull/54/changes#r1729230640
  - https://github.com/usatie/leetcode/pull/3#discussion_r1938208737
  - こういう考え方があることを知らなかった。
- 境界値テストやろうねという話。境界値を考えるクセは大事。今回であれば空配列、0が右端に最初から寄っている。0がないなど。
  - https://github.com/maxhealy01/blind-75/pull/54#discussion_r1252304534