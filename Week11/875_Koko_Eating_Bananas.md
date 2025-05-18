# 875. Koko Eating Bananas

```C++
class Solution {
public:
    // determine if the pile of bananas can be finish with specific speed
    bool canFinishAll(const vector<int>& piles, int speed, int h) {
        long totalHours = 0;
        for (int bananas : piles) {
            totalHours += (bananas + speed - 1) / speed;  // ceil(bananas / speed)
        }
        return totalHours <= h;
    }


    int minEatingSpeed(vector<int>& piles, int h) {
        int left = 1;   // slowest speed: 1 pile/hr
        int right = *max_element(piles.begin(), piles.end());   // fastest speed: max piles/hr

        // Binary search
        while (left < right) {
            int mid = left + (right - left) / 2;

            if (canFinishAll(piles, mid, h)) {
                // if the mid speed can finish, there might be a slower speed can also achieve in the left interval [left, mid]
                right = mid;
            } else {
                // if the mid speed can't finish, there might be a faster speed can also achieve in the right interval, [mid, right]
                left = mid + 1;
            }
        }

        return left;
    }
};
```