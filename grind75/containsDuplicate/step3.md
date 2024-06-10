### やろうとしていることを言語化してみる
SKIP

### 思考記録
SKIP

### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(N)

### コード
```Java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> seen = new HashSet<>();
        boolean result = false;

        for (int num : nums) {
            if (seen.contains(num)) {
                result = true;
                break;
            }

            seen.add(num);
        }

        return result;
    }
}
```
### かかった時間
1分40秒

### 感想
- こちらも仮に空配列とか渡されたどうするかという話がありそう。
  - falseが返るのはそんなに悪くなさそう。実際にそうだし。
- nullが渡されたら？
  - for文でnullポ
  - 最初にnullチェックするけど、nullが渡されない前提ならチェックしなくて良さそう
    - もし渡されたら例外投げるで良い気がする。
- Brute ForceとSortでもとけるけど、計算量を犠牲にして得るものがあまりないのでHashSetの解法だけとりあえず頭に入れておく。