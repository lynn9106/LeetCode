# 752. Open the Lock

```C++
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        if (target == "0000") return 0;

        unordered_set<string> invalid_states(deadends.begin(), deadends.end()); // record invalid states like deadends and visited states
        if (invalid_states.count("0000")) return -1;

        queue<string> q;// record current nodes
        q.push("0000");
        invalid_states.insert("0000");
        int rotate = 1;    // times of rotating

        while(!q.empty()){
            int n = q.size();        // states of current layers
            for (int i=0; i< n; i++){
                string cur = q.front();     //get current node
                q.pop();

                for (int j = 0; j<4 ; j++){     // there's 8 neighbor for s, which are only one-bit differed from s (add one or minus one) 
                    char c = cur[j];

                    // add 1 on the bit
                    cur[j] = (c == '9') ? '0' : c+1;
                    if (!invalid_states.count(cur)){
                        if(cur == target) return rotate;
                        q.push(cur);
                        invalid_states.insert(cur);
                    }

                    // minus 1 on the bit
                    cur[j] = (c == '0') ? '9' : c-1;
                    if (!invalid_states.count(cur)){
                        if(cur == target) return rotate;
                        q.push(cur);
                        invalid_states.insert(cur);
                    }

                    // restore the bit of s for next changing bit
                    cur[j] = c;
                }     
            }
            rotate++; // rotate once after one layer
        }
        return -1;
        
        
    }
};
```