# 46. Permutations

```C++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        backtrack(nums, 0, res);
        return res;
    }

private:
    void backtrack(vector<int>& nums, int start, vector<vector<int>>& res) {
        if (start == nums.size()) {
            res.push_back(nums); // push the current vector
            return;
        }

        for (int i = start; i < nums.size(); ++i) {
            swap(nums[start], nums[i]);         // decide the num of current index
            backtrack(nums, start + 1, res);    // process the next indicies
            swap(nums[start], nums[i]);         // return the order
        }
    }
};
```