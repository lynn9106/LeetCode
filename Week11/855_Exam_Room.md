# 855. Exam Room

```C++
class ExamRoom {
private:
    int N;                // total seats
    set<int> seats;       // seats that are not empty

public:
    ExamRoom(int n) {
        N = n;
    }

    int seat() {
        // Case 1: if there's no one in the examroom
        if (seats.empty()) {
            seats.insert(0);
            return 0;
        }

        int prev = -1;
        int maxDist = 0;
        int seatToSit = 0;

        // Case 2: if there's no one sit on 0
        auto it = seats.begin();
        if (*it != 0) {
            maxDist = *it;
            seatToSit = 0;
        }

        // Case 3: if we need to seat between two people
        for (auto curr : seats) {
            if (prev != -1) {
                int dist = (curr - prev) / 2;
                if (dist > maxDist) {
                    maxDist = dist;
                    seatToSit = prev + dist;
                }
            }
            prev = curr;
        }

        // Case 4: if the last person isn't on N-1
        if (*seats.rbegin() != N - 1) {
            int dist = N - 1 - *seats.rbegin();
            if (dist > maxDist) {
                seatToSit = N - 1;
            }
        }

        seats.insert(seatToSit);
        return seatToSit;
    }

    void leave(int p) {
        seats.erase(p); 
    }
};
/**
 * Your ExamRoom object will be instantiated and called as such:
 * ExamRoom* obj = new ExamRoom(n);
 * int param_1 = obj->seat();
 * obj->leave(p);
 */
```