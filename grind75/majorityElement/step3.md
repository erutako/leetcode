### やろうとしていることを言語化してみる
SKIP

### 思考記録
ムーアの投票アルゴリズムで解く

### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(1)

### コード
```Java
class Solution {
    public int majorityElement(int[] nums) {
        int candidate = Integer.MAX_VALUE;
        int count = 0;

        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }

            if (candidate == num) {
                count++;
            } else {
                count--;
            }
        }

        return candidate;
    } 
}
```
### かかった時間
1分30秒

### 感想
- 割と頭の中に解法が入った。
- しばらく寝かせて、HashMapでの解き方もよりわかりやすいコードで書く練習をする。
  - mapのmergeを使うと簡潔に書けそう。
- step2で考えた現実世界での操作にあてはめると、count == 0のときにcandidateを更新するだけでなく、count++してcontinueするというコードの方がわかりやすかもと思った。