### やろうとしていることを言語化してみる


### 思考記録
2-passでソート済み配列を大小に分けてから、targetが属す可能性がある配列をbisect_leftで二分探索する

### 計算量
#### 時間計算量
O(logN)
#### 空間計算量
O(1)

### コード
```ruby
# @param {Integer[]} nums
# @params {Integer} target
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
### かかった時間
10min

### 感想
- 書いていてもまだちょっと混乱して変なコードになることがある
  - これはどちらかというと最後に返す値がrightそのものじゃなくて、targetと比較したうえで決めた値にしないとだめとか問題の設定が他の問題と混ざってよくわからなくなることがある
  - 間違いに気づくとすぐに修正できるので、アルゴリズム自体の理解はいけてそう
- 二分探索を2つかくのはさすがに5minじゃきついかも
  - コード書いている時間がまず5minくらいかかる
