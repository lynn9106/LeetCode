# 881. Boats to Save People

```C++
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        std::sort(people.begin(), people.end());    // sort people weight in increasing order
        auto leftptr = people.begin();    // pointer from front and a pointer from back
        auto rightptr = people.end() - 1;
        int boats = 0;
        while(leftptr <= rightptr){
            // search if the boat can carry the heavy one and the light one (sum <= limit)
            if( (*leftptr + *rightptr) <= limit){   
                leftptr++;  // if yes, move both pointer to next person
            }               // if no, just move the right pointer to next person 
            rightptr--;     //(the heavy one will take the boat himself)
            boats++;
        }   // if right pointer pos <= left pointer pos, end the loop       
        return boats;
    }
};
```