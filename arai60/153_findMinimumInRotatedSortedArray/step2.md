### やろうとしていることを言語化してみる


### 思考記録
自分より右の要素がすべて大きいかどうか配列を2つに分けて判定していくイメージで実装する。
境界値をあまり意識できていないので、要素が0のときや答えが配列の末尾にあるケースを考えて書く。

### 計算量
#### 時間計算量
O(logN)
#### 空間計算量
O(1)

### コード
#### 　提出コード
```ruby
# @param {Integer[]} nums
# @return {Integer}
def find_min(nums)

    raise ArgumentError, "nums must be non-empty" if nums.nil? || nums.empty?

    left = 0
    right = nums.length - 1

    # 不変条件: 最小値は常に[left, right]にある
    # 評価式: nums[i] <= nums[right] 
    # 終了: left == rightのときに探索範囲がなくなる
    while left < right
        mid = (left + right) / 2
        if nums[mid] <= nums[right]
            right = mid
        else
            left = mid + 1
        end
    end

    nums[left]
end
```

#### 半開区間 (left, right]
``` ruby
# @param {Integer[]}
# @param {Integer}
def find_min(nums)
    left = -1
    right = nums.length - 1

    while right - left > 1
        mid = (left + right) / 2

        if nums[mid] <= nums[right]
            right = mid
        else
            left = mid
        end
    end

    nums[right]
end
```

#### 比較対象を最後尾に固定する
``` ruby
# @param {Integer[]}
# @param {Integer}
def find_min(nums)
    left = 0
    right = nums.length - 1

    while left < right
        mid = (left + right) / 2

        if nums[mid] <= nums[-1]
            right = mid
        else
            left = mid + 1
        end
    end

    nums[right]
end
```

### かかった時間
30min

### 感想
- いちから書こうとすると何をやっているかわからなくなって詰まってしまったので、Geminiといろいろやりとりした
- step1で感じた"先頭とmidと末尾の大小関係を見ていけば、ずらされた先頭がわかりそうな気持ち"は少しズレてたと理解する
  - 配列回転したときに起きることは大きい数が集まる部分配列Shighと小さい数が集まる部分配列Slowの2つにわかれることで、最小値は必ずSlow側であるからこのSlow側での最小値を見つけるというのが今回の問題
    - 回転がまったくない場合でも一番右端の値を基準に評価式に当てはめてゆけば問題ない。なぜなら、一番右端の値と比べたときに大きいか小さいかで回転の有無にかかわらず範囲の狭め方が同じだから
      - これが最大値を探す問題だと、固定すべきが左端になり逆になる。これは回転の有無によらずアンカーの値と比べたときに範囲の狭め方が同じになるようにするにはどうすればよいか考えるとわかる。
- そう考えると、step1でnums[mid] < nums[right]としていたのはかなり違和感があることに気づく
  - https://github.com/sakupan102/arai60-practice/pull/43/changes/BASE..36167fcd716b107e83476e6b2f73821e91d5554e#r1649750982
    - このコメントにもあるように配列を両端からTRUE/FALSEで塗りつぶしていると考えると、一番右端を起点にTRUEかどうかを考えてスタートしたくなるから、評価式はnums[right]を含めて評価したくなる
- 回転したときに部分配列ShighとSlowに分かれると考えると、Slowは必ず右側になるので、Slowの最大値である末尾の値と比較する評価式を使って探索範囲を狭めれば良い。
  - これは回転がない場合も探索範囲内の最大値が末尾になるので問題ない
- step1でいろいろな解き方で考えると書いたけど、最初のrightを動かしていく方式だとmidとの評価式のために常にrightをループ中に保つ必要があり、閉区間でしか解けない
  - これが配列の末尾を固定すると、rightで評価式の情報を保つ必要がなくなるので、left, rightは単なる探索範囲を表す変数になり、開区間でも解けるようになる
