### やろうとしていることを言語化してみる


### 思考記録
Linked Listを順にたどっていく。
辿っていくごとにSetにNodeをメモしていき、nextのnodeがsetに含まれているか確認する。
setに含まれていればcycleがある。
どこかでnextがnilになればcycleはないことになる。

### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(N)

### コード
```ruby
def hasCycle(head)
    visited_nodes = Set.new
    cursor = head
    while !cursor.nil?
        visited_nodes.add(cursor)
        return true if visited_nodes.include?(cursor.next)
        cursor = cursor.next
    end
    false
end
```
### かかった時間
7min

### 感想
- rubyだと最後にreturnで返すのは標準的でないようなので、最後のfalseを返すところはreturnを削除した
  - 確かにreturnが必要な箇所とそうでない箇所があるということはreturnにここで処理を終えて値を返すという意味があるということなので、処理を途中で終えるという意図がない場合はreturnを書かないほうがよさそう
- currentという名前をやめてcursorという名前にしてみた。
  - こちらのほうがnodeを辿っている感じがしてよさそう
  - https://github.com/hiroki-horiguchi-dev/leetcode/pull/1#discussion_r2281841575
- nodesという名前もvisited_nodesにしてみた。
  - 正直このくらいの長さの関数であれば個人的にはnodesいいかなという気持ちだけどvisited_nodesのほうが一目で意味がわかって良い。
- 先にSetに値を追加してから、nextを使ってcycleの確認をする実装にしてみた。
  - 先にSetで確認する実装はループ一周目で空のSetに対して1つめのノードの存在確認をしているのがちょっと違和感で、今回の方が直感的。