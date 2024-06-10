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
### かかった時間
5分45秒

### 感想
- このくらいのスコープなら変数名はそれほど問題にならないが、他のsolutionにあったseenは意味がついていて良いなと思ったので真似した。
- Solution2のコードはお遊びだけど、そこまで読みにくいわけでもないかも？否定演算子が2回出てきているところとfor文のループ継続判定の式が少し長いのが 気になる。
- 意外に時間がかかっていたので、step3で1,2分でかけるようにしておく。