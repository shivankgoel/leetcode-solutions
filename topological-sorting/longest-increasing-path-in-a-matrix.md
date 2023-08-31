```
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        int ans = 0;
        
        // t refers to total nodes in the graph
        int t = m*n; 
        vector<vector<int>> g(t, vector<int>());
        vector<int> indegree(t, 0);
        queue<int> q;
        
        vector<vector<int>> dirs{{0,1},{0,-1},{1,0},{-1,0}};
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                for(auto dir: dirs){
                    int x = i + dir[0];
                    int y = j + dir[1];
                    if(x<0 || y<0 || x>=m || y>=n) continue;
                    if(matrix[x][y] < matrix[i][j]){
                        int node1 = (x*n) + y;
                        int node2 = (i*n) + j;
                        g[node1].push_back(node2);
                        indegree[node2]++;
                        
                    }
                }
            }
        }
        
        for(int i=0; i<t; i++){
            if(indegree[i] == 0) q.push(i);
        }
        
        while(!q.empty()){
            int sz = q.size();
            ans++;
            while(sz--){
                int x = q.front(); q.pop();
                for(int nei: g[x]){
                    indegree[nei]--;
                    if(indegree[nei] == 0) q.push(nei);
                }
            }
        }
        
        return ans;
    }
};
```
