### やろうとしていることを言語化してみる


### 思考記録
step1は閉区間で解いたので、まず閉区間でもう一度解いてみる。
やりたいことはnums[i] == targetとなるようなiを見つけるか、target < nums[i]となるような最小のiを見つけること。
つまり、target <= nums[i]となるような最小のiを見つけると良さそう。
このためには配列の要素をtarget <= nums[i]となるTRUEの要素とtarget > nums[i]となるFALSEの要素にわけて最初のTRUEの要素を返せば良さそう。

### 計算量
#### 時間計算量
O(logN)
#### 空間計算量
O(1)

### コード
#### 提出コード(閉区間、mid切り下げ)
`i < left` は false、`i > right` は true（終了時 `left = right + 1`）
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search_insert(nums, target)
    left = 0
    right = nums.length - 1

    while (left <= right)
        mid = left + (right - left) / 2 # rubyだとオーバーフローは気をつけなくて良いらしいがこれでやってみる
        if (target <= nums[mid])
            right = mid - 1 # i >= mid は true 側。探索範囲の右端を mid - 1 に狭める
        else
            left = mid + 1 # i <= mid は false 側。探索範囲の左端を mid + 1 に狭める
        end
    end

    return left # 終了時 left = right + 1。i < left は false、i > right は true。left が target <= nums[i] を満たす最小の i
end
```

#### 閉区間、mid切り上げ
閉区間だとmidは切り上げでも切り下げでもいい
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search_insert(nums, target)
    left = 0
    right = nums.length - 1

    while left <= right
        mid = (left + right).ceildiv(2)
        if target <= nums[mid]
            right = mid - 1
        else
            left = mid + 1
        end
    end

    return left # 終了時 left = right + 1。i < left は false、i > right は true。left が target <= nums[i] を満たす最小の i
end
```

#### 半開区間(右が開区間)
`i < left` は false、`i >= right` は true（終了時 `left == right`）
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search_insert(nums, target)
    left = 0 # 探索範囲 [left, right) の左端（閉）
    right = nums.length # 探索範囲 [left, right) の右端（開）

    while left < right
        mid = (left + right) / 2 # 右が開区間なので、mid が必ず left <= mid < right となるように切り下げる。切り上げると最後の1要素の探索で mid == right となり範囲外になる
        if (target <= nums[mid])
            right = mid # i >= mid は true 側。探索範囲の右端を mid に狭める
        else
            left = mid + 1 # i <= mid は false 側。探索範囲の左端を mid + 1 に狭める
        end
    end

    return left # 終了時 left == right。i < left は false、i >= right は true。left が target <= nums[i] を満たす最小の i
end
```

#### 半開区間(左が開区間)
`i <= left` は false、`i > right` は true（終了時 `left == right`）
右が開区間の場合と違って、探索終了時は最後の false 側の位置で止まるので、戻り値はその右隣にする必要がある。
```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search_insert(nums, target)
    left = -1 # i <= left は false。答えが 0 のとき false 領域が空になるよう、最後の false の番兵として -1 を使う
    right = nums.length - 1 # 探索範囲 (left, right] の右端（閉）

    while left < right # 半開区間であればどちらが開区間であってもループ条件は変わらない。Search Spaceがなくなるまでループすることが条件
        mid = (left + right).ceildiv(2) # 左が開区間なので、mid が必ず left < mid <= right となるように切り上げる。切り下げると最後の1要素の探索で mid == left となり探索範囲が狭まらない
        if (target <= nums[mid])
            right = mid - 1 # i >= mid は true 側。探索範囲の右端を mid - 1 に狭める
        else
            left = mid # i <= mid は false 側。探索範囲の左端を mid に狭める
        end
    end

    return right + 1 # 終了時 left == right。i <= left は false、i > right は true。left は最後の false のインデックス（0 より前なら -1）。答えはその右隣なので right + 1
end
```

#### 開区間

```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def search_insert(nums, target)
    left = -1
    right = nums.length

    while left + 1 < right # 開区間の間に要素が一つ以上ある場合に探索を続ける
        mid = (left + right).ceildiv(2) # 開区間では最後にmidをどちらに寄せるかで無限ループすることがないので、切り上げでも切り下げでも良い
        if target <= nums[mid]
            right = mid
        else
            left = mid
        end
    end

    return right # 終了時はleft, rightが隣り合わせ。right が target <= nums[i] を満たす最小の i
end
```

#### ruby bsearch_index
実際のアプリケーションコードで書くならこうなる
```ruby
def search_insert(nums, target)
  idx = nums.bsearch_index { |x| x >= target }
  idx || nums.length
end
```
### かかった時間
10min (提出コード)

### 感想
- 今回はまず自分なりに2分探索のメンタルモデルを作ってから望んだ
  - 二分探索は閉区間、半開区間、開区間の3つの探索範囲の捉え方があって、さらに閉区間は真ん中を切り上げるか切り下げるか、半開区間は左右どちらを開区間にするかで基本的なパターンは5つある
    - この5つのパターンそれぞれ、不変条件、ループ条件、更新条件、停止条件が決まる
    - 不変条件はAnswer Spaceでループに限らず、処理を通して満たし続ける答えが存在する範囲
    - ループ条件はSearch Spaceを表しており、探索範囲が残っていること
    - 探索が終わって次の探索を始めるときに範囲をどのように狭めるか
    - 停止条件はSearch Spaceが空になることで、探索範囲が存在しないこと
  - 加えて、解きたい課題に応じて3つのことを考える必要がある
    - 初期値
    - 評価式
      - これが一番大事で二分探索はある条件に当てはまるものと当てはまらないものを二分している処理と言え、二分する式が評価式
        - [fhiyo-sanのコメント](https://github.com/Yoshiki-Iwasa/Arai60/pull/35#discussion_r1693974411)が参考になる
        - やりたいことに応じてこの評価式は変わる
        - git bisectにわたす検証式の立ち位置
        - これをどんどん適用することで配列内の要素を評価式にあてはまるTRUEの要素か、当てはまらないFALSEの要素か分けている
    - 戻り値
  - 探す対象は点と隙間の2種類あり、値それ自体を探す場合と隙間(境界線)を探す場合がある
- メンタルモデル作った後だとすっとかけた
- 各解き方で書くのはだいぶ時間がかかったが、理解が定着した
- LeetCode 35 の制約上は空配列はないが、この実装では空配列のときも0を返す
- rubyだとループの条件式のカッコは外せるからそのほうがシンプルという話もあるらしい
- midの切り捨て、rightの初期値などいろいろな要素がなぜそうなっているのかを考える切っ掛けはoda-sanのこのコメント
  - Https://github.com/seal-azarashi/leetcode/pull/39#discussion_r1849419449
