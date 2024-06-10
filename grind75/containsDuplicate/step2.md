### やろうとしていることを言語化してみる
SKIP

### 思考記録
カードに書かれた数字が既に見たことがある数字かどうか、≒与えられたカードの中でユニークな数字かどうかがわかればよいので、Setを利用する。

### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(N)

### コード
Solution1
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

Solution2
読みづらいけど、こんなコードも書けた。
Runtimeは7msでなかなか速かった。
```Java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<>();
        boolean seen = false;

        for (int i = 0; i < nums.length && !seen; i++) {
            seen = !set.add(nums[i]);
        }

        return seen;
    }
}

```

Solutionsで見つけた良さげなコード
```Java
// https://leetcode.com/problems/contains-duplicate/solutions/2459020/very-easy-100-fully-explained-c-java-python-javascript-python3-creating-set/
// Time complexity: O(n)
// Space complexity: O(n)
class Solution {
  public boolean containsDuplicate(int[] nums) {
    // Base case...
    if(nums==null || nums.length==0)
      return false;
    // Create a hashset...
    HashSet<Integer> hset = new HashSet<Integer>();
    // Traverse all the elements through the loop...
    for(int idx: nums){
      // If it contains duplicate...
      if(!hset.add(idx)){
        return true;
      }
    }
    // Otherwise return false...
    return false;
  }
}
```
### かかった時間
5分45秒

### 感想
- このくらいのスコープなら変数名はそれほど問題にならないが、他のsolutionにあったseenは意味がついていて良いなと思ったので真似した。
- Solution2のコードはお遊びだけど、そこまで読みにくいわけでもないかも？否定演算子が2回出てきているところとfor文のループ継続判定の式が少し長いのが 気になる。
- 意外に時間がかかっていたので、step3で1,2分でかけるようにしておく。
- Solution2のコードもSolutionsで見つけたコードのように書けば十分成立するし、むしろわかりやすい。
  - 拡張for文を使わない理由がないので、普通に使えばよかった。
  - 結果変数を用意せずに、パスが2つしかないのでboolean値をそれぞれでreturnすればシンプルでよい
