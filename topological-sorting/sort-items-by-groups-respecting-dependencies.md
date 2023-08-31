```
class Solution {
public:
    vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) {
        
        int t = n + (2*m);
        vector<vector<int>> g(t, vector<int>());
        vector<int> indegree(t, 0);
        stack<int> st;
        
        for(int i=0; i<n; i++){
             if(group[i] != -1){
                // Create start and end group nodes
                int groupStart = group[i]+n;
                int groupEnd = group[i]+n+m;
                g[groupStart].push_back(i);
                g[i].push_back(groupEnd);
                 indegree[i]++;
                 indegree[groupEnd]++;
            }
            for(int item: beforeItems[i]){
                // Add edge from item to i
                if(group[item] == group[i]){
                    g[item].push_back(i);
                    indegree[i]++;
                }
                else{                
                    int itemEnd = group[item] == -1? item: group[item]+n+m;
                    int iStart = group[i] == -1? i : group[i]+n;
                    g[itemEnd].push_back(iStart);
                    indegree[iStart]++;
                }
            }
        }
        
        for(int i=0; i<t; i++) if(!indegree[i]) st.push(i);
        
        vector<int> ans;
        int count = 0;
        while(!st.empty()){
            int x = st.top(); st.pop();
            if(x < n) ans.push_back(x);
            count++;
            for(int nei: g[x]){
                indegree[nei]--;
                if(indegree[nei] == 0) st.push(nei);
            }
        }
        
        return count == t? ans : vector<int>();
        
    }
};
```
