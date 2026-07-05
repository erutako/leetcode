### やろうとしていることを言語化してみる


### 思考記録
O(log n)を求められているので、2分探索なんだろうという気持ち。
ソートされた配列でなにか見つけたいときは二分探索したくなりそう。
探す対象が配列内に存在しない可能性もあるけど、配列全体を二分探索すればそれはわかりそう。

今回は必ずrotateされている前提のようなので、ShighとSlowの2つの部分は配列に分かれる模様。
midとtargetを比較したくなるんだろうなという気持ち。
targetに関する評価式を作って、それを当てはめていきたい。
評価式はいくつか考えられそうだけど、Find Minimum In Rotated Sorted Arrayで慣れたtarget <= nums[mid]で試してみる。

と思って少しコードを書いたけど、targetが見つかればすぐに返すでも良さそうなので、等しい値が見つかったときは分岐する考えもあるか。

それ以前に二分探索で解くのが難しいぞ。
配列の全体像さえ把握できれば、targetを探すのは簡単なので、ツーパスで最初に回転数を把握して、そのあとtargetを探すはできそう。

targetの評価式を当てはめる考え方が間違っていそう。
回転している状態で任意のtargetについてTRUE/FALSEでわけるのは無理そう。

ワンパスでいけそうな気はしつつ、自力では思いつかないのでとりあえずツーパスで解く。

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
def search(nums, target)
    search_high = 0
    search_low = nums.length - 1

    while search_high < search_low
        mid = (search_high + search_low) / 2
        if nums[mid] <= nums[-1]
            search_low = mid
        else
            search_high = mid + 1
        end
    end

    rotation_count = nums.length - search_low

    left = -rotation_count
    right = search_high - 1

    while left <= right

        mid = (left + right) / 2
        if target <= nums[mid]
            right = mid - 1
        else
            left = mid + 1
        end
    end

    found_index = right + 1 >= 0 ? right + 1 : right + 1 + nums.length 

    target == nums[found_index] ? found_index : -1
end
```
### かかった時間
45min

### 感想
- 謎コードを生み出してしまった
    - 一応自分はやっていることを理解できるが、他の人が見たらちんぷんかんぷんだろう
    - 選択した探索方法によって不変条件が変わるわけだが、最終的に不変条件を満たす端の値を指すときにそこから+1や-1をして探している値と対応するコードは認知負荷がやや高いので、+1や-1をせずとも一致するようにアルゴリズムを設計したほうが良さそう
- rubyの負のインデックスをアクセスを使って無理やりやろうとしているとき、2つのソート済み配列に対してそれぞれ探索すればよいと気づくべきだった
  - そうすることで、2つのソート済み配列をどう探索するかと考えるとワンパスの方法も思いつけた気がする
  - 使える情報が何かを考えて、情報の組み合わせで何ができそうか考えるクセをもう少し付けないと
  - どの情報があればどういうことができる、というのはLeetcodeで習得している部分なのだろう
- 二分探索+条件分岐というパターンで解くアルゴリズムがあると知れたのは勉強になった
- 回転を踏まえた新しいソートキーを使って比較する解法
  - https://github.com/Yoshiki-Iwasa/Arai60/pull/36#discussion_r2139249063
  - 多くの言語で配列同士を比較したときにインデックスが若い要素から順に同じインデックスの要素を比較して大小比較できることを知らなかった
    - 自分がメインで書いていたJavaはこの仕様に対応していなかった
      - Javaの場合は==が配列の等値比較でアドレス参照の比較になっているから、<や>を配列の比較に使うとアドレス参照の比較というよくわからないことになるからこの仕様になるのか
        - 逆にはPythonは==が値の比較だから、<や>を値の比較に使っても全体で整合性が取れるから問題ない
          - そう考えるとPythonでは==がアドレス比較ではないことが腑に落ちるな
            - https://github.com/MA-yo-TA/leetcode/pull/2/changes#r3448075312
- early returnをするかどうか
  - https://github.com/h1rosaka/arai60/pull/45#discussion_r2529528119
    - 私は@naoto-iwase sanと同じ感覚でearly returnしないほうが不変条件がわかりやすくよいかなという気持ち
    - early returnでΩ(1)になるので、そのメリットと不変条件の伝わりやすさのトレードオフかな
    - 多分書き方は不変条件を決める、アルゴリズムに落とす、targetが見つかったときはearly returnする、という順で書いていくことになりそうで、最後にearly returnの分岐を入れると見通しがわかりづらくなる感覚はある
    - https://github.com/h1rosaka/arai60/pull/45#discussion_r2529529754
      - このあとearly returnのコードを読んだときに思ったのはearly returnで判定した分の範囲を後続の二分探索で探索範囲から省こうとすると二分探索の処理としてはだいぶ読みづらくなるなというイメージ
      - 1ループの中に等価判定する探索と探索範囲を狭める探索があって、認知負荷が高い
      - 二分探索のお作法で探索範囲をループごとに狭めて、最終的に値があるかないかを確認したい気持ちになった
- 不変条件は否定で読んだほうがいいかも知れない話
  - https://github.com/h1rosaka/arai60/pull/45/changes/BASE..0cd8a6120e331ecc7250adf06e4d251fef1e3d4e#r2538495415
  - これはなるほどとなった。論理的に対偶なだけなんだけど、否定から捉えることでtargetがあるとき、ないときの分岐を言葉にする必要がなくなってシンプル
  - あと、探索済みの範囲と残りの探索範囲を不変条件で捉えることをしていなかったし、むしろしないほうが良いと思っていたから、探索済みかどうかで不変条件を捉えることもありなんだなと思った
- ワンパスの解法
  - ソート済み区間かどうかを判定して、ソート済みかどうかで処理を変える
  - https://github.com/sakzk/leetcode/pull/7/changes/BASE..7442eebc39f4a336f9a3a8b759b4827f37394e60#r1603034195
    - この書き方もやっていることはソート済み区間かどうかで処理を分けている
    - 単調述語でない点に違和感を持ったかもしれない
      - このアルゴリズムでも解けるけど、二分探索なら単調述語で解きたい
      - 毎ループで配列全体ではなく、配列の一部の区間に対して不変条件を適用してleft, rightを更新している
- 回転数から元の配列の並びに矯正して探索する方法
  - https://github.com/kazuki-official/leetcode/blob/fa074edebb1c5a708da6323e76bfb461bae970f6/memo.md#code1-1-binary-search
    - 自分と同じアプローチだけど、こちらのほうがわかりやすい
    - offsetを適用したときに配列長を超えるかどうかで分岐するのではなく、モジュロ演算を使えばシンプルなのか
- ソート済みでない配列が入力されたときの振る舞い
  - https://github.com/sakzk/leetcode/pull/7/changes/BASE..7442eebc39f4a336f9a3a8b759b4827f37394e60#r1602653358
  - 実際には絶対ありますよね、無限ループがデバッグ観点で避けたいというのはなるほど
  - デバッグ観点では異常を検知したらなんらか処理を止めて状況を通知してほしくて、それは例外を投げることだったり、異常が起きたことを示す戻り値だったりする
- if分岐を読むときのワーキングメモリ
  - https://github.com/sakzk/leetcode/pull/7/changes/BASE..375feba30bee21886052a7f5b13669cd5f21141d#r1603843055
  - なるほど、if-elseがネストされていれば片方の中に入ったらもう片方の条件は消せるけど、elifで続くと条件がANDで積み重なってワーキングメモリ不可が高い
- 型チェックをどうするか
  - https://github.com/Fuminiton/LeetCode/pull/43#discussion_r2133351577
  - "確かに、型が一致するか(サブタイプ含めて一致するかみるならtype、そうでないならisinstanceを選ぶ)を見る"、この視点は持っているけど言語化できていなかったので改めて型チェックの際は無意識に思いつく観点にしたい