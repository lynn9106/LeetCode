# 135. Candy

```C++
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        int total = 0;

        vector<int> candies(n, 1);     //initial give every child 1 candy
        for (int i=1; i<n; i++){    // check from left to right 
            if(ratings[i] > ratings[i-1]){     // if r>l (check right neighbor)
                candies[i] = candies[i-1] + 1;
            }
        }
        total+= candies[n-1];
        for (int i=n-2; i>=0; i--){     //check from right to left
            if(ratings[i] > ratings[i+1]){  // if l>r (check left neighbor)
                candies[i] = max(candies[i], candies[i+1] + 1);
            }
            total+= candies[i];
        }
    return total;
    }
};
```