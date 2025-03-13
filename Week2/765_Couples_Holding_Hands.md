# 765. Couples Holding Hands

```C++
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        unordered_map <int,int> pos;
        int counts = 0;
        for (int i=0 ; i<row.size(); i++){
            pos[row[i]] = i;        // <id, pos>
        }

        for (int i =0; i<row.size(); i+=2){
            int partner= (row[i]%2 == 0)? row[i]+1 : row[i]-1 ; //if partner is even or odd one
            if(row[i+1] == partner) continue;   // partner is sitting next

            // find and swap partner
            int partnerpos = pos[partner];
            swap(row[i+1], row[partnerpos]);

            // update the pos map after swapping
            pos[partner] = i+1;
            pos[row[partnerpos]] = partnerpos;
            counts++;
        }

        return counts;
        
    }
};

```