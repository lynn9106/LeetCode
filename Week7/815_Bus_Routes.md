# 815. Bus Routes

```C++
class Solution {
public:
    int numBusesToDestination(vector<vector<int>>& routes, int source, int target) {
        if (source==target) return 0;

        unordered_map<int, vector<int>> stop2Buses;
        for (int bus=0; bus< routes.size(); bus++){          // establish a stop to buses map to know which bus can try to take at the stop
            for (int stop : routes[bus]){
                stop2Buses[stop].push_back(bus);
            }
        }

        queue<int> busToTry;        // a queue for buses to try
        unordered_set<int> visitedBus;  // avoid taking same bus
        unordered_set<int> visitedStop; // avoid taking same stop

        for (int bus : stop2Buses[source]){     // start from source, put all available bus in queue
            busToTry.push(bus);
            visitedBus.insert(bus);
        }


        int totalBus = 1;
        while(!busToTry.empty()){
            int n = busToTry.size();
            for (int i=0; i<n; i++){        // check buses for this round
                int bus = busToTry.front();
                busToTry.pop();

                for (int stop: routes[bus]){
                    if (stop == target) return totalBus;    // arrvie target

                    if (visitedStop.count(stop)) continue;      // skip that the stop has been taken
                    visitedStop.insert(stop);

                    for(int nextBus: stop2Buses[stop]){         // check new bus to take from this stop
                        if (visitedBus.count(nextBus)) continue;
                        visitedBus.insert(nextBus);
                        busToTry.push(nextBus);
                    }
                }
            }
            totalBus++;
        }
        return -1; // no route to arrive since BFS is finish without arriving at target
    }
};
```