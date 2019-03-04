# 合并K个元素的有序链表
合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

**示例:**

> 输入:
[
  1->4->5,
  1->3->4,
  2->6
]<br>
输出: 1->1->2->3->4->4->5->6

# 分析
每两个链表进行一次合并，直到最后剩下一个链表

java代码如下：
```java
public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) return null;
        return mergeKLists(lists, 0, lists.length - 1);
    }
    
    private ListNode mergeKLists(ListNode[] lists, int left, int right) {
        if (left == right) return lists[left];
        int mid = left + (right - left) / 2;
        ListNode leftNode = mergeKLists(lists, left, mid);
        ListNode rightNode = mergeKLists(lists, mid + 1, right);
        return mergeTwoLists(leftNode, rightNode);
    }
    
    private ListNode mergeTwoLists(ListNode node1, ListNode node2) {
        ListNode dummy = new ListNode(0);
        merge(dummy, node1, node2);
        return dummy.next;
    }
    
    private void merge(ListNode main, ListNode node1, ListNode node2) {
        if (node1 == null) {
            main.next = node2;
            return;
        }
        if (node2 == null) {
            main.next = node1;
            return;
        }
        if (node1.val < node2.val) {
            main.next = node1;
            merge(main.next, node1.next, node2);
        } else {
            main.next = node2;
            merge(main.next, node1, node2.next);
        }
    }
```

也可以使用一个优先队列，将所有链表头结点添加进去，根据val的大小进行排序。每次返回队首元素，并判断该链表下一节点是否为空，不为空则放入优先队列，直至优先队列为空。

java代码如下：
```java
public ListNode mergeKLists(ListNode[] lists) { 
        PriorityQueue<ListNode> queue = new PriorityQueue<>((a,b)->a.val-b.val);
        for(ListNode node:lists)
            if(node != null)
                queue.offer(node);
        if(queue.isEmpty()) return null;
        ListNode root = new ListNode(1);
        ListNode p = root;
        while(!queue.isEmpty()){
            ListNode node = queue.poll();
            p.next = node;
            p = p.next;
            if(node.next != null)
                queue.offer(node.next);
        }
        return root.next;
    }
```
