# 2366. Minimum Replacements to Sort the Array

```C++
class Solution {
public:
    long long minimumReplacement(vector<int>& nums) {
        long long counts = 0;
        int maximum = nums.back();

        for(int i=nums.size()-2; i>=0 ; i--){
            if( nums[i] <= maximum){
                maximum = nums[i];
            }
            else{
                int part = (nums[i] + maximum - 1) / maximum;
                int divide = part - 1;
                
                counts += divide;
                maximum = nums[i]/part;
            }
        }
        return counts;
    }
};

```