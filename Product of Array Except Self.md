# Product of Array Except Self
    给定长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

**示例:**

> 输入: [1,2,3,4]<br>
> 输出: [24,12,8,6]

说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。

**进阶：**

你可以在常数空间复杂度内完成这个题目吗？（出于对空间复杂度分析的目的，输出数组不被视为额外空间。）
# 分析
  * 不许使用乘法，所以将该元素左右两边的数分别遍历相乘。但时间复杂度要求O(n)，所以我们可以构造两个数组。
  * 假设nums={a,b,c,d}，则left={1,a,ab,abc},right={bcd,cd,d,1}.
  * 很显然left*right即为结果.

**java代码如下：**
```
public int[] productExceptSelf(int[] nums) {
        int len = nums.length;
        int[] left = new int[len];
        int[] right = new int[len];
        
        left[0] = 1;
        for(int i=1;i<len;i++){
            left[i] = left[i-1] * nums[i-1];
        }
        
        right[len-1] = 1;
        for(int i=len-2;i>=0;i--){
            right[i] = right[i+1] * nums[i+1];
        }
        
        for(int i=0;i<len;i++){
            left[i] *= right[i];
        }
        
        return left;
    }
```
> 很显然空间复杂度达不到常数的要求。此时我们可以定义一个临时变量记录反向的乘积，然后将结果直接记录在输出数组中。
```
public int[] productExceptSelf(int[] nums) {
        int len = nums.length;
        int[] output = new int[len];
        
        output[0] = 1;
        for(int i=1;i<len;i++){
            output[i] = output[i-1] * nums[i-1];
        }
        
        int a = 1;//临时变量来记录反向的乘积
        for(int i=len-2;i>=0;i--){
             a *= nums[i+1];
            output[i] *= a;
        }
        
        return output;
    }
```
