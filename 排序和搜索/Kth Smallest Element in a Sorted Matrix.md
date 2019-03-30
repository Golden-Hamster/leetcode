# Kth Smallest Element in a Sorted Matrix
给定一个 n x n 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第k小的元素。
请注意，它是排序后的第k小元素，而不是第k个元素。

**示例:**

> matrix = [<br>
   [ 1,  5,  9],<br>
   [10, 11, 13],<br>
   [12, 13, 15]<br>
],<br>
k = 8,<br>
返回 13。

**说明:** 
你可以假设 k 的值永远是有效的, 1 ≤ k ≤ n2 。

# 分析

首先没想到什么好办法，只好将二维数组放在一个一维数组里面，然后采用快排改变的寻找第k小的数[quickSelect算法](https://github.com/Golden-Hamster/leetcode/blob/master/%E6%8E%92%E5%BA%8F%E5%92%8C%E6%90%9C%E7%B4%A2/%E6%91%86%E5%8A%A8%E6%8E%92%E5%BA%8FII.md)。将二维数组改成一维数组复杂度为O(n^2)，quickSelect算法时间复杂度为O(n).

java代码如下：
```java
    public int kthSmallest(int[][] matrix, int k) {
        int ver = matrix.length;
        int hor = matrix[0].length;
        int number = ver * hor;
        int[] nums = new int[number];
        int m = 0;
        for(int i=0;i<ver;i++)
            for(int j=0;j<hor;j++)
                nums[m++] = matrix[i][j];
        return quickSelect(nums, k-1, 0, nums.length-1);
    }
    
    public int quickSelect(int[] nums, int k, int left, int right){
        if(left >= right)
            return nums[left];
        int mid = getMiddle(nums, left, right);
        if(mid > k && left < mid)
            return quickSelect(nums, k, left, mid-1);
        else if(mid < k && right > mid)
            return quickSelect(nums, k, mid+1, right);
        return nums[k];
    }
    
    public int getMiddle(int[] nums, int left, int right){
        int temp = nums[left];
        while(left < right){
            for(;left<right && nums[right]>temp;right--);
            nums[left] = nums[right];
            for(;left<right && nums[left]<=temp;left++);
            nums[right] = nums[left];
        }
        nums[left] = temp;
        return left;
    }
```

提交后看了网上的解法，找到最小值（左上角）与最大值（右下角）的中值，然后在二维数组中找小于中值的数的数量，若此数量大于等于k，则将最大值变为中值，否则将最小值变为中值+1.直到最后最小值等于最大值，第k小的数就是最小值（最大值）。

java代码如下：
```java
public int kthSmallest(int[][] matrix, int k) {
        int ver = matrix.length;
        int hor = matrix[0].length;
        int left = matrix[0][0], right = matrix[ver-1][hor-1];
        while(left < right){
            int mid = left + (right - left)/2;
            if(getMiddle(matrix, mid) >= k)
                right = mid;
            else
                left = mid + 1;
        }
        return left;
    }
    
    int getMiddle(int[][] nums, int mid){
        int res  = 0, j = 0, i = nums.length-1;
        while(i>=0&&j<nums[0].length){
            if(nums[i][j] > mid)
                i--;
            else{
                res += i + 1;
                j++;
            }
        }
        return res;
    }
```
