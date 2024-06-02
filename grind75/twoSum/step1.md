初見ではないものの、時間が空いてしまったので再度STEP1からやり直す。

### やろうとしていることを現実世界で考えて言語化する
数字が書かれたカードが何枚か(nums)と、壁に書かれた一つの数字(target)がある。
2枚のカードを選んで書かれている数字を足し算すると壁に書かれた数字となるようなカードの番号を見つける。
そのようなカードの組み合わせは一つしか存在せず、同じカードを2回選ぶことはできない。

### 解法について考えていることを記録する
1枚のカードを固定して、残りのカードを1枚ずつ見て、足すと壁に書かれた数字になる組み合わせを探す。
残り5分なので一旦この方針でコードを書く。

### 計算量について考える
2 <= nums.length <= 10^4だから、O(n^2)でも許してもらえそう。
多分ほんとはもっと少ない計算量でいける。

### コード
```Java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] indices = new int[2];
        boolean isPairFound = false;
        for (int i = 0; i < nums.length; i++) {
            if (isPairFound) {
                break;
            }
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    indices[0] = i;
                    indices[1] = j;
                    isPairFound = true;
                    break;
                }
            }
        }
        return indices;
    }
}
```