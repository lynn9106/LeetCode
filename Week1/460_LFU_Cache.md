# 460_LFU_Cache

```C++
struct Node{
    int key;
    int value;
    int counter;
    Node* prev;
    Node* next;
    Node(int k, int v):key(k), value(v), prev(nullptr), next(nullptr), counter(1){};
};

class DLinkedList{
public:
    Node* head;
    Node* tail;
    int size;
    DLinkedList(){
        head = new Node(0,0);
        tail = new Node(0,0);
        head->next = tail;
        tail->prev = head;
        size = 0;
    }

    void addNode(Node* node){
        head->next->prev = node;
        node->prev = head;
        node->next = head->next;
        head->next = node;
        size++;
    }

    void deleteNode(Node* node){
        node->next->prev = node->prev;
        node->prev->next = node->next;
        size--;
    }

    Node* removeTail(){
        if (size>0){
            Node* node = tail->prev;
            deleteNode(node);
            return node;
        }
        return nullptr;
    }
};

class LFUCache {
private:
    int capacity;
    int minFreq; // for recording the least frequency

    unordered_map<int, Node*> cache; // find node by key
    unordered_map<int, DLinkedList*> freqMap; // same freq will be in one DL list, find DL list by freq

    void update(Node* node){
        int freq = node->counter;
        // delete node from the origin list
        freqMap[freq]->deleteNode(node);

        // if node freq is minFreq and its list is empty, minfreq become the next freq
        if(freq == minFreq && freqMap[freq]->size == 0){
            minFreq++;
        }

        node->counter++;    //the node is recently used, add freq

        // if new freq DL list isn't exist, establish the new list
        if(freqMap.find(node->counter) == freqMap.end()){
            freqMap[node->counter] = new DLinkedList();
        }

        freqMap[node->counter]-> addNode(node);
    }

public:
    LFUCache(int capacity) {
        this->capacity = capacity;
        this->minFreq = 0;
    }
    
    int get(int key) {
        // if key isn't exist
        if (cache.find(key)==cache.end()){  
            return -1;
        }
        // if key exist
        else{
            Node* node = cache[key];
            update(node);
            return node->value;
        }
    }
    
    void put(int key, int value) {
        // if key isn't exist
        if (cache.find(key) == cache.end()){
            if(cache.size() >= capacity){
                DLinkedList* LFUlist = freqMap[minFreq];
                Node* LFUnode = LFUlist->removeTail();
                cache.erase(LFUnode->key);
                delete LFUnode;
            }

            Node* node = new Node(key, value); // create the new node
            minFreq = 1;
            if(freqMap.find(minFreq) == freqMap.end()){
                freqMap[1] = new DLinkedList();
            }
            freqMap[minFreq]->addNode(node);
            cache[key] = node;
        }
        //if key is exist, update the node
        else{
            Node* node = cache[key];
            node->value = value;
            update(node);
        }

    }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache* obj = new LFUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */

```