# 42. Trapping Rain Water

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if (n == 0) return 0;

        int left = 0, right = n - 1;
        int leftMax = 0, rightMax = 0;
        int water = 0;

        // the water storage capacity of each position, is determined by min(maxleft, maxright) - current height

        while (left < right) {
            if (height[left] < height[right]) {    // if the left side is lower than right side, the height is decided by leftMax
                if (height[left] >= leftMax) {      // update leftMax
                    leftMax = height[left];
                } else {
                    water += leftMax - height[left];        // stroage water
                }
                left++;
            } else { // if the right side is lower than left side, the height is decided by rightMax
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    water += rightMax - height[right];
                }
                right--;
            }
        }

        return water;
    }
};

```