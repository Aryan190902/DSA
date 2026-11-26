**Question**: [LRU Cache](https://leetcode.com/problems/lru-cache/)

**Approach**:
- Use `unordered_map` for average $O(1)$ key lookup and a doubly linked list to track recent usage in $O(1)$ pointer time.
- The linked list is ordered from most recently used at `head->next` to least recently used at `tail->prev`.
- `get` moves an existing node to the front and returns its value; a missing key returns `-1`.
- `put` updates and promotes an existing key, or inserts a new node at the front.
- When the cache is full, remove the node immediately before `tail`, erase its key from `unordered_map`, and delete the node.
- The capacity is guarded explicitly so a cache of size zero never attempts to remove `head` as though it were a cache entry.

TC: Average $O(1)$ per `get` and `put`; $O(n)$ worst case per operation if hash collisions degrade an operation. Linked-list insertion and removal take $O(1)$.
SC: $O(n)$ for the at most `capacity` nodes and map entries.

**Code**:
```cpp
class Node{
public:
    int key;
    int val;
    Node *prev;
    Node *next;
    Node(int key, int val){
        this->key = key;
        this->val = val;
        this->prev = NULL;
        this->next = NULL;
    }
};

class LRUCache {
private:
    unordered_map<int, Node*> mp;

    // Dummy boundaries keep every real node between head and tail.
    Node* head = new Node(-1,-1);
    Node* tail = new Node(-1,-1);

    void remove(Node* node){
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void addToFront(Node* node){
        // Insert immediately after head: this node is now most recent.
        head->next->prev = node;
        node->next = head->next;

        head->next = node;
        node->prev = head;
    }

public:
    int cap;
    LRUCache(int capacity) {
        head->next = tail;
        tail->prev = head;
        this->cap = capacity;
    }

    int get(int key) {
        if(mp.count(key)){
            Node* node = mp[key];

            // A successful lookup makes this key the most recently used.
            remove(node);
            addToFront(node);
            return node->val;
        }
        return -1;
    }

    void put(int key, int value) {
        if(this->cap == 0){
            return;
        }

        // Existing keys only update value and usage position.
        if(mp.count(key)){
            Node* node = mp[key];
            node->val = value;
            remove(node);
            addToFront(node);
            return;
        }

        // Evict the node immediately before tail when the cache is full.
        if(mp.size() == this->cap){
            Node* node = tail->prev;
            remove(node);
            mp.erase(node->key);
            delete node;
        }

        Node* node = new Node(key, value);
        addToFront(node);
        mp[key] = node;
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```