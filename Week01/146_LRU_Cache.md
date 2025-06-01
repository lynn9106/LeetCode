# 146 LRU Cache

```C++
class LRUCache {
private:
    int capacity;

    struct Node{
        int value;
        int key;
        Node* prev;
        Node* next;
        Node(int k, int v):key(k), value(v), prev(nullptr), next(nullptr){}
    };

    unordered_map<int, Node*> cache; //For finding key in O(1)
    Node* head;
    Node* tail;

    void moveToHead(Node* node){        //Move the recently used node to the head
        deleteNode(node);
        addNode(node);
    }

    Node* removeTail(){        //Move the LRU(tail) node
        Node* node = tail->prev;
        deleteNode(node);
        return node;    //return to release memory
    }

    void deleteNode(Node* node){
        node->next->prev = node->prev;
        node->prev->next = node->next;
    }

    void addNode(Node* node){
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }

public:
    LRUCache(int capacity) {
        this->capacity = capacity;
        head = new Node(0,0);   //Dummy head: for pointing the RU
        tail = new Node(0,0);   //Dummy tail: for pointing the LRU
        head->next = tail;      // the RU node
        tail->prev = head;      // the LRU node
    }
    
    int get(int key) {
        if(cache.find(key) == cache.end()){ // can't find the key
            return -1;
        }
        // find key, get the value, and move the node to head
        Node* node = cache[key];
        moveToHead(node);
        return node->value;
    }
    
    void put(int key, int value) {
        if(cache.find(key) == cache.end()){   // can't find key
             // over capacity, remove tail(LRU), delete key and release
            if(cache.size() >= capacity){  
                Node* LRU = removeTail();
                cache.erase(LRU->key);
                delete LRU;
            } 
            // create node, add in cache and add to head
            Node* RU = new Node(key, value);
            cache[key] = RU;
            addNode(RU);

        }
        else{ // find key, get node, update value, move to head
            Node* node = cache[key];
            node->value = value;
            moveToHead(node);
        }
        
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */

```