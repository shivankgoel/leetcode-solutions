```
class Solution {
public:
    int largestPathValue(string colors, vector<vector<int>>& edges) {
        
        int n = colors.size();
        int ans = 1;
        
        vector<int> indegree(n,0);
        vector<vector<int>> g(n,vector<int>());
        queue<int> q;
        for(auto e: edges){
            g[e[0]].push_back(e[1]);
            indegree[e[1]]++;
        }
        for(int i=0; i<n; i++) if(indegree[i] == 0) q.push(i);
        
        vector<vector<int>> dp(n, vector<int>(26,0)); 
        int count = 0;
        while(q.size()){
            int x = q.front(); q.pop();
            dp[x][colors[x]-'a']++;
            
            //x is processed completely it's color value won't change now
            // Below syntax is equal to for(int color=0; color<26; color++) ans = max(ans, dp[x][color]);
    
            ans = max(ans, *max_element(dp[x].begin(), dp[x].end()));
            
            count++;
            for(int nei: g[x]){
                
                for(int color=0; color<26; color++) 
                    dp[nei][color] = max(dp[nei][color], dp[x][color]);
                
                indegree[nei]--;
                if(indegree[nei] == 0) q.push(nei);
            }
        }
        
        return count == n? ans : -1;
        
    }
};
```
