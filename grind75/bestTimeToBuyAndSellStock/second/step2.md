### やろうとしていることを言語化してみる


### 思考記録


### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(1)

### コード
```ruby
# @param {Integer[]} prices
# @return {Integer}
def max_profit(prices)
    max_profit = 0
    min_price = Float::INFINITY

    prices.each do |p|
        min_price = [p, min_price].min
        max_profit = [max_profit, p - min_price].max
    end

    max_profit
end
```
### かかった時間
5min

### 感想
- step1よりも変数名を具体化してみた
- if文があるとif文の条件を頭に入れながらコードを読むのが少し大変に感じたので、min, maxで実装してみた
- 問題文を読むとlength=1は想定されているけど、0は想定されていなさそうなので空配列のときは例外投げたほうが呼び出し側は嬉しいかも