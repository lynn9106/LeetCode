# 895. maximum-frequency-stack

```C++
class FreqStack {
    unordered_map<int, int> freq;              // 各數字出現次數
    unordered_map<int, vector<int>> stack;     // 頻率f次的數值, 依照push的順序排列
    int maxFreq = 0;                           // 目前最高頻率
public:
    FreqStack() = default;

    /* 將 val 推入 stack*/
    void push(int val) {
        int f = ++freq[val];                   // 更新 val 的頻率
        maxFreq = max(maxFreq, f);             // 更新目前最高頻率
        stack[f].push_back(val);               // 按頻率 f 進入對應freq stack
    }

    /* pop最常出現、且最接近stack最後的元素 */
    int pop() {
        int val = stack[maxFreq].back();      // 取出目前最高頻率且最後插入的元素
        stack[maxFreq].pop_back();

        if (stack[maxFreq].empty())            // 若此頻率沒有元素了
            --maxFreq;                         // 最高頻率 -= 1

        --freq[val];                           // val 的頻率 -= 1
        return val;                  
    }
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(val);
 * int param_2 = obj->pop();
 */
```