### やろうとしていることを言語化してみる
SKIP

### 思考記録
SKIP

### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(1)

### コード
```Java
class Solution {
  public int maxProfit(int[] prices) {
    int minPrice = prices[0];
    int maxProfit = 0;

    for (int i = 1; i < prices.length; i++) {
      if (prices[i] < minPrice) {
        minPrice = prices[i];
      } else {
        maxProfit = Math.max(maxProfit, prices[i] - minPrice);
      }
    }

    return maxProfit;
  }
}
```
### かかった時間
4分

### 感想
- 変数名といい、コードの書き方といい、自分が現実世界でイメージしている動作を割とコードで再現できた気がする。
- 慣れたら1,2分くらいで書けそうなので、すぐに現実世界での操作をイメージしてコードに落とせるようにしたい。