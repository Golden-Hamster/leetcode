# 四数相加 II
给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。

**例如:**

> 输入:
A = [ 1, 2]<br>
B = [-2,-1]<br>
C = [-1, 2]<br>
D = [ 0, 2]<br>

> 输出:
2

**解释:**
    
    两个元组如下:
    1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
    2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0

# 分析
最先想到的就是暴力破解了。当然，时间复杂度达到O(n^4)，提交超时是肯定的（笑）。想了半天没什么好的优化方法，网上找了一下，结果都是分成两组来计算。也就是说，AB一组，CD一组。将AB计算的值与次数存在HashMap中，计算CD时，如果HashMap中存在计算结果的相反数，就将HashMap中存的次数加起来。最后就得出了结果。时间复杂度为O(n^2)。

java代码如下：
```java
public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        int len = A.length;
        if(len == 0) return 0;
        int count = 0;
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<len;i++){
            for(int j=0;j<len;j++){
                int num = A[i] + B[j];
                if(map.containsKey(num))
                    map.put(num, map.get(num)+1);
                else
                    map.put(num, 1);
            }
        }
        for(int i=0;i<len;i++){
            for(int j=0;j<len;j++){
                int num = -C[i] - D[j];
                if(map.containsKey(num))
                    count += map.get(num);
            }
        }
        return count;
    }
```
