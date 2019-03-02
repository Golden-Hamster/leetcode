# Sliding Window Maximum
给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口 k 内的数字。滑动窗口每次只向右移动一位。

返回滑动窗口最大值。

**示例:**

> 输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3<br>
输出: [3,3,5,5,6,7] 

**解释:** 

>   滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7

**注意：**

你可以假设 k 总是有效的，1 ≤ k ≤ 输入数组的大小，且输入数组不为空。

**进阶：**

你能在线性时间复杂度内解决此题吗？
# 分析
最直接的方法，先从第一个窗口中找到最大值，然后每一次滑动比较进来的数如果大于最大值，则更新最大值，出去的数为最大值并且进来的数小于最大值，则重新寻找最大值。其他情况最大值不变。

java代码如下：
```java
public int[] maxSlidingWindow(int[] nums, int k) {
        int len = nums.length;
        if(len == 0) return new int[0];
        int[] output = new int[len-k+1];
        int max = findMax(nums,0,k-1);
        output[0] = max;
        for(int i=1,j=k;j<len;i++,j++){
            if(nums[j] > max)
                max = nums[j];
            else if(nums[i-1]==max && nums[j]<max)
                max = findMax(nums,i,j);
            output[i] = max;
        }
        return output;
    }
    
    public int findMax(int[] nums, int start, int end){
        int max = Integer.MIN_VALUE;
        for(int i=start;i<=end;i++){
            max = Math.max(max,nums[i]);
        }
        return max;
    }
```

再就是使用一个双向队列,里面存储数组的下标。每一次删去队列的头结点即要被移出的结点（满足i-k的索引），然后从尾部循环删去比要进来的那个数小的结点，确保队列的降序。这样头结点就是最大的值所在的索引。然后将其加入到输出数组中。

java代码如下：
```java
public int[] maxSlidingWindow(int[] nums, int k) {
        int len = nums.length;
        if(len == 0) return new int[0];
        int[] output = new int[len-k+1];
        Deque<Integer> deque = new ArrayDeque<>();
        for(int i=0;i<len;i++){
            if(!deque.isEmpty() && deque.peekFirst() == i-k) deque.poll();
            while(!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) deque.pollLast();
            deque.offerLast(i);
            if(i+1>=k) output[i+1-k] = nums[deque.peek()];
        }
        return output;
    }
```

扫描一次即可，时间复杂度为O(n)
