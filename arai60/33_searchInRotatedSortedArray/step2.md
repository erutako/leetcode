### やろうとしていることを言語化してみる


### 思考記録
2つのソート済み配列のどちらかにtargetがある可能性があり、targetがある可能性がある方を二分探索する

### 計算量
#### 時間計算量
O(logN)
#### 空間計算量
O(1)

### コード
#### 提出コード
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    min_index = find_min_index(nums)

    left, right = if nums[min_index] <= target && target <= nums[-1]
                    [min_index, nums.length]
                  else
                    [0, min_index]
                  end

    while left < right
        mid = (left + right) / 2
        if target <= nums[mid]
            right = mid
        else
            left = mid + 1
        end
    end

    nums[right] == target ? right : -1
end

private

# @param {Integer[]} nums
# @return {Integer}
def find_min_index(nums)
    left = 0
    right = nums.length

    while left < right
        mid = (left + right) / 2
        if nums[mid] <= nums[-1]
            right = mid
        else
            left = mid + 1
        end
    end

    right
end
```

#### 2-pass 2つのソート済み配列に分ける 半開区間 (left, right]
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    min_index = find_min_index(nums)

    left, right = if nums[min_index] <= target && target <= nums[-1]
                    [min_index - 1, nums.length - 1]
                  else
                    [-1, min_index - 1]
                  end

    while left < right
        mid = (left + right + 1) / 2 # 切り下げだと動かない
        if target <= nums[mid]
            right = mid - 1
        else
            left = mid
        end
    end

    nums[right + 1] == target ? right + 1 : -1
end

private

# @param {Integer[]} nums
# @return {Integer}
def find_min_index(nums)
    left = -1
    right = nums.length - 1

    while left < right
        mid = (left + right + 1) / 2
        if nums[mid] <= nums[-1]
            right = mid - 1
        else
            left = mid
        end
    end

    right + 1
end
```

#### 2-pass 2つのソート済み配列に分ける 閉区間
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    min_index = find_min_index(nums)

    left, right = if nums[min_index] <= target && target <= nums[-1]
                    [min_index, nums.length - 1]
                  else
                    [0, min_index - 1]
                  end

    while left <= right
        mid = (left + right) / 2 # 切り上げでも動く
        if target <= nums[mid]
            right = mid - 1
        else
            left = mid + 1
        end
    end

    nums[right + 1] == target ? right + 1 : -1
end

private

# @param {Integer[]} nums
# @return {Integer}
def find_min_index(nums)
    left = 0
    right = nums.length - 1

    while left <= right
        mid = (left + right) / 2
        if nums[mid] <= nums[-1]
            right = mid - 1
        else
            left = mid + 1
        end
    end

    right + 1
end
```

#### 2-pass 2つのソート済み配列に分ける 開区間
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    min_index = find_min_index(nums)

    left, right = if nums[min_index] <= target && target <= nums[-1]
                    [min_index - 1, nums.length]
                  else
                    [-1, min_index]
                  end

    while right - 1 > left
        mid = (left + right) / 2 # 切り上げでも動く
        if target <= nums[mid]
            right = mid
        else
            left = mid
        end
    end

    nums[right] == target ? right : -1
end

private

# @param {Integer[]} nums
# @return {Integer}
def find_min_index(nums)
    left = -1
    right = nums.length

    while right - 1 > left
        mid = (left + right) / 2
        if nums[mid] <= nums[-1]
            right = mid
        else
            left = mid
        end
    end

    right
end
```

#### 2-pass offset 閉区間
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    offset = find_min_index(nums)

    array_left = 0
    array_right = nums.length - 1
    while array_left <= array_right
        array_mid = (array_left + array_right) / 2 # 切り上げでも動く
        sorted_mid = (array_mid + offset) % nums.length
        if nums[sorted_mid] < target
            array_left = array_mid + 1
        else
            array_right = array_mid - 1
        end
    end

    sorted_candidate_index = (array_right + 1 + offset) % nums.length
    nums[sorted_candidate_index] == target ? sorted_candidate_index : -1
end

private

# @param {Integer[]} nums
# @return {Integer}
def find_min_index(nums)
    left = 0
    right = nums.length - 1

    while left <= right
        mid = (left + right) / 2
        if nums[mid] <= nums[-1]
            right = mid - 1
        else
            left = mid + 1
        end
    end

    right + 1
end
```

#### 2-pass offset 半開区間 [left, right)
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    offset = find_min_index(nums)

    array_left = 0
    array_right = nums.length
    while array_left < array_right
        array_mid = (array_left + array_right) / 2 # 切り上げだと動かない
        sorted_mid = (array_mid + offset) % nums.length
        if nums[sorted_mid] < target
            array_left = array_mid + 1
        else
            array_right = array_mid
        end
    end

    sorted_candidate_index = (array_right + offset) % nums.length
    nums[sorted_candidate_index] == target ? sorted_candidate_index : -1
end

private

# @param {Integer[]} nums
# @return {Integer}
def find_min_index(nums)
    left = 0
    right = nums.length

    while left < right
        mid = (left + right) / 2
        if nums[mid] <= nums[-1]
            right = mid
        else
            left = mid + 1
        end
    end

    right
end
```

#### 2-pass offset 半開区間 (left, right]
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    offset = find_min_index(nums)

    array_left = -1
    array_right = nums.length - 1
    while array_left < array_right
        array_mid = (array_left + array_right + 1) / 2 # 切り下げだと動かない
        sorted_mid = (array_mid + offset) % nums.length
        if nums[sorted_mid] < target
            array_left = array_mid
        else
            array_right = array_mid - 1
        end
    end

    sorted_candidate_index = (array_right + 1 + offset) % nums.length
    nums[sorted_candidate_index] == target ? sorted_candidate_index : -1
end

private

# @param {Integer[]} nums
# @return {Integer}
def find_min_index(nums)
    left = -1
    right = nums.length - 1

    while left < right
        mid = (left + right + 1) / 2
        if nums[mid] <= nums[-1]
            right = mid - 1
        else
            left = mid
        end
    end

    right + 1
end
```

#### 2-pass offset 開区間
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    offset = find_min_index(nums)

    array_left = -1
    array_right = nums.length
    while array_right - array_left > 1
        array_mid = (array_left + array_right) / 2 # 切り上げでも動く
        sorted_mid = (array_mid + offset) % nums.length
        if nums[sorted_mid] < target
            array_left = array_mid
        else
            array_right = array_mid
        end
    end

    sorted_candidate_index = (array_right + offset) % nums.length
    nums[sorted_candidate_index] == target ? sorted_candidate_index : -1
end

private

# @param {Integer[]} nums
# @return {Integer}
def find_min_index(nums)
    left = -1
    right = nums.length

    while right - 1 > left
        mid = (left + right) / 2
        if nums[mid] <= nums[-1]
            right = mid
        else
            left = mid
        end
    end

    right
end
```

#### 1-pass ソート済み判定&範囲内判定 閉区間 mid切り下げ
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    left = 0
    right = nums.length - 1
    
    while left < right
        mid = (left + right) / 2
        if nums[left] <= nums[mid]
            if nums[left] <= target && target <= nums[mid]
                right = mid
            else
                left = mid + 1
            end
        else
            if nums[mid] < target && target <= nums[right]
                left = mid + 1
            else
                right = mid
            end
        end
    end

    nums[left] == target ? left : -1
end
```

#### 1-pass ソート済み判定&範囲内判定 閉区間 mid切り上げ
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    left = 0
    right = nums.length - 1
    
    while left < right
        mid = (left + right + 1) / 2
        if nums[left] <= nums[mid]
            if nums[left] <= target && target < nums[mid]
                right = mid - 1
            else
                left = mid
            end
        else
            if nums[mid] <= target && target <= nums[right]
                left = mid
            else
                right = mid - 1
            end
        end
    end

    nums[right] == target ? right : -1
end
```

#### 1-pass ソート済み判定&範囲内判定 半開区間 [left, right)
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    left = 0
    right = nums.length
    
    while right - left > 1
        mid = (left + right) / 2 # 切り上げでも動く
        if nums[left] <= nums[mid]
            if nums[left] <= target && target < nums[mid]
                right = mid
            else
                left = mid
            end
        else
            if nums[mid] <= target && target <= nums[right - 1]
                left = mid
            else
                right = mid
            end
        end
    end

    nums[left] == target ? left : -1
end
```

#### 1-pass ソート済み判定&範囲内判定 半開区間 (left, right]
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    left = -1
    right = nums.length - 1
    
    while right - left > 1
        mid = (left + right) / 2 # 切り上げでも動く
        if nums[left + 1] <= nums[mid]
            if nums[left + 1] <= target && target <= nums[mid]
                right = mid
            else
                left = mid
            end
        else
            if nums[mid] < target && target <= nums[right]
                left = mid
            else
                right = mid
            end
        end
    end

    nums[right] == target ? right : -1
end
```

#### 1-pass ソート済み判定&範囲内判定 開区間 (left, right) mid切り下げ
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    left = -1
    right = nums.length
    
    while right - left > 2
        mid = (left + right) / 2
        if nums[left + 1] <= nums[mid]
            if nums[left + 1] <= target && target <= nums[mid]
                right = mid + 1
            else
                left = mid
            end
        else
            if nums[mid] < target && target <= nums[right - 1]
                left = mid
            else
                right = mid + 1
            end
        end
    end

    nums[left + 1] == target ? left + 1 : -1
end
```

#### 1-pass ソート済み判定&範囲内判定 開区間 (left, right) mid切り上げ
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
    left = -1
    right = nums.length
    
    while right - left > 2
        mid = (left + right + 1) / 2
        if nums[left + 1] <= nums[mid]
            if nums[left + 1] <= target && target < nums[mid]
                right = mid
            else
                left = mid - 1
            end
        else
            if nums[mid] <= target && target <= nums[right - 1]
                left = mid - 1
            else
                right = mid
            end
        end
    end

    nums[right - 1] == target ? right - 1 : -1
end
```

#### 1-pass 複合key 閉区間
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
  last = nums.last
  
  priority = ->(x) { [x <= last ? 1 : 0, x] }
  target_key = priority[target]

  left = 0
  right = nums.length - 1
  while left <= right
    mid = (left + right) / 2 # 切り上げでも動く
    if ((priority[nums[mid]] <=> target_key) == -1)
      left = mid + 1
    else
      right = mid - 1
    end
  end

  nums[right + 1] == target ? right + 1 : -1
end
```

#### 1-pass 複合key 半開区間 [left, right)
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
  last = nums.last
  
  priority = ->(x) { [x <= last ? 1 : 0, x] }
  target_key = priority[target]

  left = 0
  right = nums.length
  while left < right
    mid = (left + right) / 2 # 切り上げだと動かない
    if (priority[nums[mid]] <=> target_key) == -1
      left = mid + 1
    else
      right = mid
    end
  end

  nums[right] == target ? right : -1
end
```

#### 1-pass 複合key 半開区間 (left, right]
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
  last = nums.last
  
  priority = ->(x) { [x <= last ? 1 : 0, x] }
  target_key = priority[target]

  left = -1
  right = nums.length - 1
  while left < right
    mid = (left + right + 1) / 2 # 切り下げだと動かない
    if (priority[nums[mid]] <=> target_key) == -1
      left = mid
    else
      right = mid - 1
    end
  end

  nums[right + 1] == target ? right + 1 : -1
end
```

#### 1-pass 複合key 開区間
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
  last = nums.last
  
  priority = ->(x) { [x <= last ? 1 : 0, x] }
  target_key = priority[target]

  left = -1
  right = nums.length
  while right - left > 1
    mid = (left + right) / 2 # 切り上げでも動く
    if (priority[nums[mid]] <=> target_key) == -1
      left = mid
    else
      right = mid
    end
  end

  nums[right] == target ? right : -1
end
```

#### 1-pass 複合key bsearch
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search(nums, target)
  last = nums.last
  priority = ->(x) { [x <= last ? 1 : 0, x] }
  target_key = priority[target]

  candidate_index = nums.bsearch_index { |x| (priority[x] <=> target_key) >= 0 }
  nums[candidate_index] == target ? candidate_index : -1
end
```

### かかった時間
10min(提出コード)

### 感想
- step1でだいぶ理解を深めたので、すっとかけた。
- 1-passは述語がループごとに変わるのがやはり認知負荷が高く、レビューしづらい。2-passは述語が固定されるのでアルゴリズムとしてわかりやすいと思う。
- 複合keyの実装はoda-sanのideaを参考にRubyで実装
  - https://github.com/Yoshiki-Iwasa/Arai60/pull/36#discussion_r1712955053
- 色々コードを見ていて4パターンあるなぁと思って各探索方法で書いてみると、midの切り上げ・切り下げで動く・動かないの判断がすぐできるようになった
  - Answer Spaceの捉え方と、Answer Spaceをどう確定させているか、Search Spaceをどう狭めているかがわかればだいたい分かる
    - これがわかれば空配列など境界値の入力のときに正しく動くかなども瞬時にわかる気がする
  - 今回は"1-pass ソート済み判定&範囲内判定"の解法のAnswer Spaceの捉え方がbisect_leftと違っていて最初分かりづらかった
