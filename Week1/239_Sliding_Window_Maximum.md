# 146 LRU Cache

```C++
#include <deque>
using namespace std;

class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> dq;
        vector<int> result;
            for(int i= 0; i<nums.size(); i++ ){

                // if current number is larger than the element at
                while(!dq.empty() && dq.back() < nums[i]){
                    // pop out the smaller number in dq
                    dq.pop_back();
                }

                // put current number in dq
                dq.push_back(nums[i]);

                // check if the number out of window in this round is the maximum number, then pop out the element
                if(i > k-1 && nums[i-k] == dq.front()){
                    dq.pop_front();
                }

                // add the maximum number of the window to res
                if(i >= k-1){
                    result.push_back(dq.front());
                }

            }
    return result;
    }
};
```