### 課題の言語化(できたら現実世界で同じ課題を再現する)
step1,2でできたためSKIP

### 思考記録
HashMapでstep1,2の課題の言語化をイメージしながら実装する。
さっきSTEP2を終えたばかりなので、すぐに手が動く。

### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(N)

### コード
```Java
class Solution {
  public int[] twoSum(int[] nums, int target) {
    HashMap<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
      int requiredVal = target - nums[i];
      if (map.containsKey(requiredVal)) {
        return new int[]{i, map.get(requiredVal)};
      }
      map.put(nums[i], i);
    }

    // 値が見つからなかったとき
    return new int[]{-1, -1};
  }
}
```
### かかった時間
5分26秒

### 感想(他の方のコードなどを読んで)
- HashTableよりHashMapのほうが1ms早くなった。このあたりも仕様を把握して使い分けできるようにしたい。
- IDEになれているので、配列の初期化でSyntaxErrorを出したりして時間を使った。数をこなせばSyntaxErrorでつまずくところは減らしてスピードを上げられそう。
- 今日は短期記憶で頭の中に解答のコードが入ってしまったので、しばらく寝かせてまた解いてみる。
