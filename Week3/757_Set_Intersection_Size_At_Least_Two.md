# 757. Set Intersection Size At Least Two

```C++
class Solution {
public:
    int intersectionSizeTwo(vector<vector<int>>& intervals) {
        // sort the end number in increasing order, if same sort by the start number in decreasing
        sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b) {
            if (a[1] == b[1])
                return a[0] > b[0];
            return a[1] < b[1];
        });
        
        int res = 0;
        int a = -1, b = -1;         // a,b represent the last two number of the set, a<b
        
        for (int i=0; i<intervals.size(); i++) {
            const auto& interval = intervals[i];
            int l = interval[0], r = interval[1];
            // if l>b, the interval has no number chosen, choose 2 num: a=r-1 b=r
            if (l > b) {
                res += 2;
                a = r - 1;
                b = r;
            }
            // if l>a and l<=b, we need one more number
            else if (l > a) {
                res += 1;
                a = b;
                b = r;
            }
            // else we have two number, check next interval
        }
        
        return res;
    }
};
```