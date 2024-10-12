### やろうとしていることを言語化してみる
数字が書かれたカードが並んでいる。
並べられたカードの部分列の中で、その和が最大となるものを求める。
最初から順にカードを見ていき、連続するカードの和をメモする。
連続するカードの和は負になったとき、連続して加算することをその時点でリセットし、次のカードから新たに和を計算してまたメモをする。
連続するカードの和の最大値は別途メモしておき、その時点での連続するカードの和の最大値が、これまでの最大値より大きい場合、最大値を更新する。

### 思考記録
シーケンシャルにカードを見ていくのが一番早そう。

### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(1)

### コード
```Java
class Solution {
  public int maxSubArray(int[] nums) {
    int max = Integer.MIN_VALUE;
    int currentSum = 0;
    
    for (var num : nums) {
      currentSum += num;
      
      if (max < currentSum) {
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

``` JavaScript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    let max = Number.MIN_SAFE_INTEGER;
    let currentSum = 0;
    
    for (let i = 0, len = nums.length; i < len; i++) {
        let num = nums[i];
        currentSum += num;
        
        if (currentSum > max) {
            max = currentSum;
        }
        
        if (currentSum < 0) {
            currentSum = 0;
        }
    }
    
    return max;
};
```

### かかった時間
11分

### 感想
- すごく久々に解いたが、思っていたより時間がかからなかった。
- DPを意識して解けていないので、DPを念頭にどのように答えを出そうとしているかを言語化できるようになる。
- JavaScriptでも書いてみる。
```chatinput

```