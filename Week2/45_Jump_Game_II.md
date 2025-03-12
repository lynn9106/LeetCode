# 45. Jump Game II

```C++
class Solution {
public:
    int jump(vector<int>& nums) {
        int jump = 0, range_farest = 0, farest = 0;
        for (int i = 0; i < nums.size()-1 ; i++){

            farest = max(farest, i+nums[i]);
            if(i == range_farest){
                jump++;
                range_farest = farest;
            }

        } 
        return jump;
    }
};


```