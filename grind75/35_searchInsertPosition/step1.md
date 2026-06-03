### やろうとしていることを言語化してみる


### 思考記録
重複がないソート済み整数配列が渡されて、対象の値を探す。
対象の値がない場合はその値を重複がないソート済み配列になるように挿入したときのindexを返す。

ソート済みなので二分探索が有効そう。
ベースケースにたどり着いたときに、対象の値かどうかで返す値が変わる。
対象の値じゃないときは、挿入箇所が最後のmidより大きいか、小さいかで変わりそう。

### 計算量
#### 時間計算量
O(logN)
#### 空間計算量
O(logN)

### コード
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search_insert(nums, target)
    left = 0
    right = nums.length - 1

    return binary_search_insert(left, right, nums, target)
end

# @param left [Integer] 探索範囲の開始インデックス
# @param right [Integer] 探索範囲の終了インデックス
# @param nums [Array<Integer>] 昇順にソートされた整数の配列
# @param target [Integer] 検索および挿入の対象となる値
# @return [Integer] ターゲットのインデックス、または挿入すべきインデックス
def binary_search_insert(left, right, nums, target)
    if right < left
        return left
    end

    mid = (left + right) / 2 
    
    return mid if target == nums[mid]

    if target < nums[mid]
        binary_search_insert(left, mid - 1, nums, target)
    else
        binary_search_insert(mid + 1, right, nums, target)
    end
end
```
### かかった時間
15min

### 感想
- これで一旦検証してみるかと思ったコードがAcceptされた
  - 全てのパターンを想定できていたようで良かった
- でも、まだ理解がふわふわしている
  - https://github.com/seal-azarashi/leetcode/pull/38#discussion_r1836463140
    - この議論は不変条件を考えることがなぜ必要で、考えたうえで二分探索するとなぜやりたいことができるのかを言語化するヒントになる
  - https://discord.com/channels/1084280443945353267/1192736784354918470/1199018938005213234
    - この例がすごくわかりやすかった
  - 自分で何パターンかかけるようになって、それぞれがなぜ正しく動くのかを人に説明できるところまでがこの問題のゴールかな
- 空間計算量がコールスタックでO(logN)になることに最初気づけなかった
- rubyには[bsearch](https://docs.ruby-lang.org/ja/latest/method/Array/i/bsearch.html)という2分探索するメソッドがあるらしい
  - これは流石に使わないけど、実装は読んでみる
- 問題では想定されていないけど、空配列が渡されても大丈夫な実装に偶然なっていた