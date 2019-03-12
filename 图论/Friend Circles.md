# Friend Circles
班上有 N 名学生。其中有些人是朋友，有些则不是。他们的友谊具有是传递性。如果已知 A 是 B 的朋友，B 是 C 的朋友，那么我们可以认为 A 也是 C 的朋友。所谓的朋友圈，是指所有朋友的集合。

给定一个 N * N 的矩阵 M，表示班级中学生之间的朋友关系。如果M[i][j] = 1，表示已知第 i 个和 j个学生互为朋友关系，否则为不知道。你必须输出所有学生中的已知的朋友圈总数。

**示例 1:**

> **输入:** <br>
[ [1,1,0],<br>
  [1,1,0],<br>
  [0,0,1] ]<br>
**输出:** 2 <br>
**说明:** 已知学生0和学生1互为朋友，他们在一个朋友圈。第2个学生自己在一个朋友圈。所以返回2。

**示例 2:**

> **输入:**<br>
[[1,1,0],<br>
 [1,1,1],<br>
 [0,1,1]]<br>
**输出:** 1 <br>
**说明:** 已知学生0和学生1互为朋友，学生1和学生2互为朋友，所以学生0和学生2也是朋友，所以他们三个在一个朋友圈，返回1。

**注意：**

* N 在[1,200]的范围内。
* 对于所有学生，有M[i][i] = 1。
* 如果有M[i][j] = 1，则有M[j][i] = 1。

# 分析


使用一个标记数组boolean[]friends表示是否标记过此人。遍历n个人（0-n-1），若feiends[i]为false则表示此人没遍历过，广度优先搜索遍历此人的朋友，若此人的朋友有没遍历的，标记上继续遍历此人朋友的朋友，依次下去

java代码如下：
```java
public int findCircleNum(int[][] M) {
        int len = M.length;
        if(len == 0) return 0;
        int count = 0;
        boolean[] friends = new boolean[len];
        for(int i=0;i<len;i++){
            if(!friends[i]){
                dfs(M, friends, i);
                count++;
            }  
        }
        return count; 
    }
    public void dfs(int[][] M, boolean[] friends, int i){
            friends[i] = true;
            for(int j=0;j<M.length;j++){
                if(!friends[j] && M[i][j] == 1 && i != j){
                    dfs(M, friends, j);
                }
            }
    }
```
