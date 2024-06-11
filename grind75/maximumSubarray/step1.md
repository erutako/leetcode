### やろうとしていることを言語化してみる
数字が書かれたカードが左から右に順番に1行で並んでいる。
連続するカードに書かれた数字の和が最大になるようなカードの並びの部分を見つける。

一番左のカードから順に見ていく。
紙に常にある時点までの最大値をメモしておく。
つまり、1枚目が2であればまずは2を最大値としてメモし、2枚目が3であれば5をメモ、3枚目が-1であれば最大値のメモは5のままにする。

加えて、ある時点での連続するカードの和もメモしておく。
つまり、1枚目が2であれば2をメモし、2枚目が3であれば5をメモ、3枚目が-1であれば4をメモする。
ここで、ある時点での連続するカードの和が0以下になったとき、その時点での和は無視して次のカードから再度和を計算する。

なぜなら、ある時点までの連続するカードの和を実は1枚のカードだったと仮定したとき(先述の例で言えば1-3枚目のカードが1枚の4と書かれたカードと考える)、仮定のカードに書かれた数字が正である限り、後続のカードに書かれた値との和が最大値を更新する可能性があるが、仮定のカードに書かれた数字が負である場合はそのカードを含まないほうが後続のカードに書かれた和の最大値が大きくなることが明らかだからである。
~~また、仮定のカードが0の場合は後続のカードに書かれた和の最大値を計算するにあたって無視しても問題ない。
というわけで、ある時点での連続するカードの和が0以下になったときは、その時点での和は無視して次のカードから仕切り直して和を計算して問題ない。~~
→コードに書いてみた気づいたけど、0のときは無視してもよいのだが、いっぽうで継続してもよいので、今回は継続パターンで書く。そのほうが読みやすい。
無視するということをコードに書こうとすると1つ処理が加わるはずなので、そう考えると0のときは継続したほうがシンプルなのかも。

### 思考記録
Kadane's Algorithmの問題だ。
順番を変えられないのでソートはだめ。
HashMapもあんまり使えなさそう。
Brute Forceはできそう。
1 <= nums.lengthなので、問題的には空配列は考慮しなくて良い。
-10^4 <= nums[i] <= 10^4なので、負の値はありうる。

同じ和になるsubarrayが複数ある場合は、どちらのsubarrayであってもその和が把握できていればよい。

### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(1)

Solution1
### コード
```Java
class Solution {
    public int maxSubArray(int[] nums) {
        int max = Integer.MIN_VALUE;
        int currentSum = 0;

        for (int num : nums) {
            currentSum += num;

            if (currentSum > max) {
                max = currentSum;
            }

            if (currentSum < 0) {
                currentSum = 0;
            }
        }

        return max;
    }
}
```

Solution2
```Java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums == null) {
            throw new NullPointerException("配列がnullです。");
        }
        if (nums.length == 0) {
            throw new IllegalArgumentException("配列の要素数が0です。");
        }

        int max = nums[0];
        int currentSum = nums[0];

        for (int i = 1; i < nums.length; i++) {
            if (currentSum < 0) {
                currentSum = 0;
            }

            currentSum += nums[i];
            if (currentSum > max) {
                max = currentSum;
            }
        }

        return max;
    }
}
```

### かかった時間
思考記録までで15分。
そのあと8分30秒でコードを書けた。

### 感想
- 配列に負の数しかないケースを考えられていなかった。最大値判定の前に現在の和が0より小さいかどうかを判定していたが最大の和が0より小さいことはあり得るので、先に最大値を判定しなければならなかった。
- 今回の問題の前提ではありえないが、空配列が渡されたときに実際に取りうる値であるInteger.MIN_VALUEが返るのはよくないので例外を投げるのが良さそう。
    - 例外投げるパターンでSolution2を書いてみた。
    - maxの初期値もnums[0]で初期化すれば、与えられた配列の中で取りうる値から外れることはないので良さそう。
        - このパターンで書くと、今度はloop内で先にcurrentSum < 0の判定をして、その後currentSum += nums[i], currentSum > maxにしないと[-2, 1]みたいなときに1単独で最大和とするケースを取りこぼす。
- 多分もっとカードとメモでやるときの動作のイメージを具体化した方が良い。
    - まずは1枚目に書かれた数字をこれまでの最大値と現在の和としてメモしておけば、カードに書かれた数字しか使うことがないので安心
    - 2枚目以降はまず現在の和が負かどうかを判定すべき。負であれば現在の和は使わないという切り分けができて、これはまず最初にすべき。
    - その後、現在の和を更新して最大値を更新すると良さそう。
    - そう考えるとSolution1は最初に最大値を更新するかどうかを考えているので、Acceptedだとしても良くないコードな気がする。
- Solution1でint max = Integer.MIN_VALUEは仕様上考えられる最小値を設定しており、先に空配列チェックをしておけば問題ないので、このままで良いかも。
- 他のSolutionを見て気づいたけど、最大値の比較はMath.maxを使ったほうが読みやすかった。
- 分割統治法でも解けるらしい。たしかに。計算量がO(NlogN)になるので、単純な全探索よりは速いけど、直感的でないし時間計算量がKadane's Algorithmよりも大きいので今回は採用しなくて良さそう。
  - https://leetcode.com/problems/maximum-subarray/solutions/1595186/java-kadane-divide-and-conquer-dp/
  - ↑で紹介されているが、動的計画法でも解ける。このほうがKadane'sより直感的かも。