# 摆动排序 II
给定一个无序的数组 nums，将它重新排列成 nums[0] < nums[1] > nums[2] < nums[3]... 的顺序。

**示例 1:**

> **输入:** nums = [1, 5, 1, 1, 6, 4]<br>
**输出:** 一个可能的答案是 [1, 4, 1, 5, 1, 6]

**示例 2:**

> **输入:** nums = [1, 3, 2, 2, 3, 1]<br>
**输出:** 一个可能的答案是 [2, 3, 1, 3, 1, 2]

**说明:**
你可以假设所有输入都会得到有效的结果。

**进阶:**
你能用 O(n) 时间复杂度和 / 或原地 O(1) 额外空间来实现吗？

# 分析

简单的方法，就是先对数组进行排序，然后将排序后的数组分为left与right两部分，分别从尾部根据奇偶数索引填充nums数组。当然，进阶的条件一个都达不到，时间复杂度为O(nlogn),空间复杂度为O(n).

java代码如下：
```java
public void wiggleSort(int[] nums) {
            if (nums == null || nums.length == 1) return;
            int[] temp = Arrays.copyOf(nums, nums.length);
            Arrays.sort(temp);
            int m = (temp.length - 1) / 2, n = temp.length - 1;
            for (int i = 0; i < nums.length; i++) {
                nums[i] = (i % 2) == 0 ? temp[m--] : temp[n--];
            }
    }
```

## 进阶

我们可以不用排序，只要知道中位数就行。这里采用快速排序的思想，其中getMiddle与快排中没有什么不同，变化的只是quickSelect中的递归。只需判断k与getMiddle中返回的索引即可，时间复杂度为O(n)。函数quickSelect就可以找出中位数，其中，参数k表示第k小的数。

找到中位数后，为了使用O(1)的空间将数字按索引奇偶插入进去，这里参照[荷兰国旗问题](https://en.wikipedia.org/wiki/Dutch_national_flag_problem#Pseudocode)（也叫三切分），index函数将索引0,1,2,3,4,5...映射为1,3,5,0,2,4。

java代码如下：
```java
public void wiggleSort(int[] nums) {
        if(nums.length == 0) return;
        int len = nums.length;
        int mid = quickSelect(nums, len/2, 0, len-1);
        int i=0, start=0, end=len-1;
        while(i <= end){
            if(nums[index(i, len)] > mid)
                swap(nums, index(i++, len), index(start++, len));
            else if(nums[index(i, len)] < mid)
                swap(nums, index(i, len), index(end--, len));
            else
                i++;
        }
    }
    
    public void swap(int[] nums, int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    
    public int index(int i, int len){
        return (i * 2 + 1)%(len | 1);
    }
    //寻找中位数
    public int quickSelect(int[] nums, int k, int left, int right){
        if(left == right)
            return nums[left];
        int mid = getMiddle(nums, left, right);
        if(k < mid && left < mid)//中位数包含在left到mid-1；
            return quickSelect(nums, k, left, mid - 1);
        if(k > mid && right > mid)//中位数包含在mid+1到right
            return quickSelect(nums, k, mid + 1, right);
        return nums[mid];//中位数
    }
    
    public int getMiddle(int[] nums, int left, int right){
        int temp = nums[left];
        while(left < right){
            for(;left<right && nums[right] > temp;right--);
            nums[left] = nums[right];
            for(;left<right && nums[left] <= temp;left++);
            nums[right] = nums[left];
        }
        nums[left] = temp;
        return left;
    }
```
