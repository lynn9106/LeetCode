# 435. Non-overlapping Intervals

```C++
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.empty()) return 0;

        // sort the intervals according to their end time
        sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[1] < b[1];
        });

        int count = 0; // the intervels number to remove
        int prev_end = intervals[0][1]; // record the end time of previous interval, ffrom the first interval

        for (int i = 1; i < intervals.size(); ++i) {    // start from second interval
            if (intervals[i][0] < prev_end) {       // if there's overlap, remove this interval
                count++;
            } else {
                prev_end = intervals[i][1];     // no overlap, update the perv_end
            }
        }

        return count;
    }
};
```