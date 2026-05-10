### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(1)

### コード
```ruby
# @param {Integer[]} prices
# @return {Integer}
# @raise [ArgumentError]
def max_profit(prices)
    if prices.nil? || prices.empty?
        raise ArugmentError, "株価データ（prices）が空です。計算には少なくとも1日以上のデータが必要です。"
    end

    min_price = Float::INFINITY
    max_gain = 0

    prices.each do |p|
        min_price = [min_price, p].min
        max_gain = [max_gain, p - min_price].max
    end

    max_gain
end
```
### かかった時間
5min

### 感想
- profitではなくgainを使ってみた
  - https://github.com/TakayaShirai/leetcode_practice/pull/36/changes#r3036507957
  - 関数名を変えるとleetcode上の実行でエラーになるので、関数名はprofitのまま
- 問題の前提に沿わない入力は例外をraiseするようにしてみた