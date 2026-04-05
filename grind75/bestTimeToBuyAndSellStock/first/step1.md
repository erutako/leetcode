### やろうとしていることを言語化してみる
数字が書かれたカードが最大で10^5枚ある。
カードは順番に並んでいる。
若い番目のカードの数字-遅い番目のカードの数字で最大の数を探す。
若い番目のカードの数字-遅い番目のカードの数字が1以上にならないとき、0を答えにする。

### 思考記録
固定したり、ソートしたりすると良さそうという感覚を持つ。
若い番目-遅い番目は動かせない条件なので、その前提で考える。
若い番目を固定して、後の番目を順に見ていき最大値を探すという作業を最後から1つ手前の番目まで繰り返せばとりあえず最大値は見つかりそう。

O(N^2)の以下コードでSubmitすると、Time Limit Exceededになる。

### 計算量
#### 時間計算量
O(N^2)
#### 空間計算量
O(1)

### コード
```Java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        for (int i = 0; i < prices.length; i++) {
            int buyingPrice = prices[i];
            for (int j = i + 1; j < prices.length; j++) {
                int sellingPrice = prices[j];
                int profit = sellingPrice - buyingPrice;
                if (profit > maxProfit) {
                    maxProfit = profit;
                }
            }
        }

        return maxProfit;
    }
}
```
### かかった時間
15分

### 感想
- 全探索のコードはまず思いついたけど、pricesの長さが増えてくると厳しかった。10^5でも厳しいのか。このあたりJavaだとNの大きさと計算量でざっくりどのくらい時間がかかるかがよくわかっていない
- Discordのコメントで、処理を分解してそれぞれを1重ループでできると理解したあとに、それぞれの処理を1つのループにまとめるというアプローチはなるほどなと思った。
- https://leetcode.com/problems/best-time-to-buy-and-sell-stock/solutions/3914105/most-optimized-solution-easy-to-understand-c-java-python/
  - この説明がわかりやすかった。
  - ある時点での最大利益を順に走査するという考え方。
    - ある時点での最大利益はその時点での価格-それまでの最小価格で、それまでの最小価格は順に走査していく過程で更新していくことができる。
- [Kadane's Algorithm](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/solutions/4868897/most-optimized-kadane-s-algorithm-java-c-python-rust-javascript/) に解法が似ているらしい。
  - 確かに似ている