### やろうとしていることを言語化してみる
人間がナンバーTシャツを着て、手を繋いでいて、左隣の人とだけ情報交換できる。
隣の人が同じ数字のTシャツを着ていたら、隣の人に更に隣の人を連れてきてもらって、隣の人は手つなぎの輪から外れてもらう。
隣の人が違う数字のTシャツを着ていたら、自分の隣はあなたで決まりだから、今度はあなたが私がいままでやっていたことをして、と言って仕事を任せる。
隣の人がいなくなったら仕事が終わりで、重複が排除されている。
これをするには先頭に人がいることを保証する必要がある。

### 思考記録
シンプルに頭から重複排除していく実装をまずやる。


### 計算量
#### 時間計算量
O(N)
#### 空間計算量
O(!)

### コード
#### 提出コード
```ruby
def delete_duplicates(head)
    return head if head.nil?

    dedup_tail = head
    while dedup_tail.next
        if dedup_tail.val == dedup_tail.next.val
            dedup_tail.next = dedup_tail.next.next
        else
            dedup_tail = dedup_tail.next
        end
    end

    head
end
```

#### 再帰

```ruby
def delete_duplicates(head)
    return head if head.nil? || head.next.nil?

    if head.val == head.next.val
        delete_duplicates(head.next)
    else
        head.next = delete_duplicates(head.next)
        head
    end
end
```

#### 二重ループ
```ruby
def delete_duplicates(head)
    cursor = head
    while cursor
        scanner = cursor.next
        while scanner && cursor.val == scanner.val
            scanner = scanner.next
        end
        cursor.next = scanner
        cursor = scanner
    end

    head
end
```
### かかった時間
提出コード 5min

### 感想
- 番号がついたTシャツを着ている人が手を繋いでいる様子を想像するとすぐにコードがかけた
  - 自然とコーナーケースも考慮するようになった
    - 一人もいない場合など
  - 人間がやるならどうするかを考えるのは本当に大事
- 二重ループでもかけるらしいので、ちょっと考えて書いてみた
  - 見た目はO(N^2)だけど実際はO(N)
