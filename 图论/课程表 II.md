#  课程表 II
现在你总共有 `n`门课需要选，记为`0`到`n-1`。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: `[0,1]`

给定课程总量以及它们的先决条件，返回你为了学完所有课程所安排的学习顺序。

可能会有多个正确的顺序，你只要返回一种就可以了。如果不可能完成所有课程，返回一个空数组。

**示例 1:**

> **输入:** 2, [[1,0]]<br>
**输出:** [0,1]<br>
**解释:** 总共有 2 门课程。要学习课程 1，你需要先完成课程0。因此，正确的课程顺序为 [0,1] 。

**示例 2:**

> **输入:** 4, [[1,0],[2,0],[3,1],[3,2]]<br>
**输出:** [0,1,2,3] or [0,2,1,3]<br>
**解释:** 总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。因此，一个正确的课程顺序是 [0,1,2,3] 。另一个正确的排序是 [0,2,1,3] 。

**说明:**

1. 输入的先决条件是由边缘列表表示的图形，而不是邻接矩阵。详情请参见[图的表示法](https://blog.csdn.net/woaidapaopao/article/details/51732947)。
2. 你可以假定输入的先决条件中没有重复的边。

**提示:**

1. 这个问题相当于查找一个循环是否存在于有向图中。如果存在循环，则不存在拓扑排序，因此不可能选取所有课程进行学习。
2. [通过 DFS 进行拓扑排序](https://www.coursera.org/specializations/algorithms) - 一个关于Coursera的精彩视频教程（21分钟），介绍拓扑排序的基本概念。
3. 拓扑排序也可以通过 [BFS](https://baike.baidu.com/item/%E5%AE%BD%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2/5224802?fr=aladdin&fromid=2148012&fromtitle=%E5%B9%BF%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2) 完成。

# 分析

如果做过[课程表](https://github.com/Golden-Hamster/leetcode/blob/master/%E5%9B%BE%E8%AE%BA/%E8%AF%BE%E7%A8%8B%E8%A1%A8.md)，这道题就非常简单了。就是在课程表的基础上，把在广度优先搜索中课程进入队列的顺序输出到一个数组中就是结果

java代码如下：
```java
public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] count = new int[numCourses];
        Queue<Integer> queue = new LinkedList<>();
        int[] res = new int[numCourses];
        int j = 0;
        
        for(int i=0;i<prerequisites.length;i++)
            count[prerequisites[i][0]]++;
        for(int i=0;i<numCourses;i++)
            if(count[i] == 0){
                queue.offer(i);
                res[j++] = i;
            }   
        while(!queue.isEmpty()){
            int n = queue.poll();
            for(int i=0;i<prerequisites.length;i++)
                if(prerequisites[i][1] == n && --count[prerequisites[i][0]] == 0){
                    queue.offer(prerequisites[i][0]);
                    res[j++] = prerequisites[i][0];
                }
        }
        for(int i=0;i<numCourses;i++)
            if(count[i] != 0)
                return new int[0];
        return res;
    }
```
