### やろうとしていることを言語化してみる


### 思考記録
色々書いてみて開区間が今回のやりたいことに近いという気持ちになったので開区間で書く。

### 計算量
#### 時間計算量
O(logN)

#### 空間計算量
O(1)

### コード
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search_insert(nums, target)
    left = -1
    right = nums.length

    while left + 1 < right
        mid = (left + right) / 2
        if target <= nums[mid]
            right = mid
        else
            left = mid
        end
    end

    right
end
```
### かかった時間
3min

### 感想
- だいぶ何をしているかを意識してかけるようになってきた
- それでもまだ細かいところであれこれどっちにすればいいんだっけ？正しく理解できていないところがあるので、しばらくbinary searchの問題を解いて頭に定着させる
- rubyとして不自然じゃない書き方も今回は意識した