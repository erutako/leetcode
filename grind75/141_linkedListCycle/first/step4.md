鳩の巣原理のマジックナンバーを解消する
```ruby
# @param {ListNode} head
# @return {Boolean}
def hasCycle(head)
    cursor = head
    max_nodes_allowed = 10_000
    1.upto(max_nodes_allowed) do |_n|
        return false if cursor.nil?
        cursor = cursor.next
    end
    cursor ? true : false
end
```