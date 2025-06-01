# 50. Pow(x, n)

```C++
class Solution {
public:
    double myPow(double x, int n) {
        long long N = n;
        if (N<0){
            x = 1.0/x;
            N = -N;
        }

        double num = 1;

        while(N>0){
            if(N%2 == 1){
                num = num * x;
            }
            x = x*x;
            N = N/2;
        }

        return num;
    }
};
```