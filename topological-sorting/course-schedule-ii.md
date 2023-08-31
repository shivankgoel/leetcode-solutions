```
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        
        g = defaultdict(list)
        indeg = [0] * numCourses
        
        for u,v in prerequisites:
            g[v].append(u)
            indeg[u]+=1
        
        q = []
        for i in range(numCourses):
            if indeg[i] == 0: q.append(i)
        
        ans  = []
        while q:
            cur = q.pop()
            ans.append(cur)
            for nei in g[cur]:
                indeg[nei]-=1
                if indeg[nei] == 0: q.append(nei)
        
        return ans if len(ans) == numCourses else []
```
