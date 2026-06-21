### やろうとしていることを言語化してみる


### 思考記録
配列の末尾と比較して、その値以下の最小値を求めたい問題と捉える。

### 計算量
#### 時間計算量
O(logN)
#### 空間計算量
O(1)

### コード
#### 提出コード
```ruby
# @param {Integer[]} nums
# @return {Integer}
def find_min(nums)

    raise ArgumentError, "nums must be non-empty" if nums.nil? || nums.empty?

    left = 0
    right = nums.length - 1

    # 不変条件: leftより左側はnums[-1]より大きい、rightの右側はnums[-1]以下
    # 評価式: nums[i] <= nums[-1]
    # ループ条件: [left, right]に2つ以上の要素が含まれる 
    while left < right
        mid = (left + right) / 2

        if nums[mid] <= nums[-1]
            right = mid
        else
            left = mid + 1
        end
    end

    nums[left]
end
```

#### 半開区間 [left, right)
```ruby
# @param {Integer[]} nums
# @return {Integer}
def find_min(nums)
    left = 0
    right = nums.length

    # 不変条件: leftより左側はnums[-1]より大きい、rightの右側はnums[-1]以下、ただし配列の末尾より後ろのインデックスを仮想的に定義したとき、末尾より後ろの値はnums[-1]以下とみなす。
    # 評価式: nums[i] <= nums[-1]
    # ループ条件: [left, right)に1つ以上の要素が含まれる 
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

#### 半開区間 (left, right]
```ruby
# @param {Integer[]} nums
# @return {Integer}
def find_min(nums)
    left = -1
    right = nums.length - 1

    # 不変条件: leftの左側はnums[-1]より大きい、rightの右側はnums[-1]以下、ただし配列の先頭より前のインデックスを仮想的に定義したとき、先頭より前の値はnums[-1]より大きいとみなす。
    # 評価式: nums[i] <= nums[-1]
    # ループ条件: (left, right]に2つ以上の要素が含まれる 
    while right - left > 1
        mid = (left + right) / 2

        if nums[mid] <= nums[-1]
            right = mid
        else
            left = mid
        end
    end

    nums[right]
end
```

#### 開区間 (left, right)
```ruby
# @param {Integer[]} nums
# @return {Integer}
def find_min(nums)
    left = -1
    right = nums.length

    # 不変条件: leftの左側はnums[-1]より大きい、rightの右側はnums[-1]以下、ただし配列の末尾より後ろのインデックスを仮想的に定義したとき、末尾より後ろの値はnums[-1]以下とみなし、
    #          配列の先頭より前のインデックスを仮想的に定義したとき、先頭より前の値はnums[-1]より大きいとみなす。
    # 評価式: nums[i] <= nums[-1]
    # ループ条件: (left, right)に1つ以上の要素が含まれる 
    while right - left > 1
        mid = (left + right) / 2

        if nums[mid] <= nums[-1]
            right = mid
        else
            left = mid
        end
    end

    nums[right]
end
```

#### 組み込みライブラリを使う
```ruby
# @param {Integer[]} nums
# @return {Integer}
def find_min(nums)
    nums.bsearch {|x| x <= nums[-1]}
end
```

### かかった時間
5min

### 感想
- 配列の末尾と比較して、最小値を探す問題だと理解すると二分探索もすんなりかけるようになった
  - 配列の末尾と比較するとなぜうまくいくかは、配列が先頭から最大値までと最小値から末尾までの2つに分かれる、または配列全体で回転が起きていない状態の2つしかないと理解すると腑に落ちた
- 末尾を固定することで開区間でも解けるようになって、やっていることがわかりやすい気がする。
