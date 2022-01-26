# LRU Cache

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

* LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
* int get(int key) Return the value of the key if the key exists, otherwise return -1.
* void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.

The functions get and put must each run in O(1) average time complexity.

## My solution

```Java
class LRUCache {
    
    class Node {
        int key;
        int value;
        Node prev;
        Node next;
    }
    
    private int capacity;
    private int currentCapacity;
    HashMap<Integer, Node> hm;
    private Node head, tail;
    
    private void addNode(Node n) {
        //H -> T
        n.prev = head;
        // H -> N; H -> T; H.next is still T, T.prev is still H
        n.next = head.next;
        // H -> N -> T; H -> T; N now points to T (aka head.next)
        head.next.prev = n;
        // H -> N -> T; H -> T; head.next is T, so we're setting T.prev to N
        head.next = n;
        //head.next finally points to N
        // H -> N -> T
    }
    
    private void removeNode(Node n) {
        //have the node before N point to the node after N and vice versa
        Node prev = n.prev;
        Node next = n.next;
        prev.next = next;
        next.prev = prev;
    }
    
    private void moveNodeToHead(Node n) {
        //The add node method adds a new node to the head, so by removing and re-adding
        //we can effectively just move the node to the head
        removeNode(n);
        addNode(n);
    }
    
    private Node removeLRUNode() {
        //Since new nodes get added to the head, the tail is the least-recently accessed node
        Node n = tail.prev;
        removeNode(n);
        return n;
    }

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.currentCapacity = 0;
        this.hm = new HashMap<Integer, Node>();
        head = new Node();
        tail = new Node();
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        if (hm.containsKey(key)) {
            Node n = hm.get(key);
            moveNodeToHead(n);
            return n.value;
        }
        else {
            return -1;
        }
    }
    
    public void put(int key, int value) {
        if (!hm.containsKey(key)) {
            Node newNode = new Node();
            newNode.key = key;
            newNode.value = value;
            hm.put(key, newNode);
            addNode(newNode);
            currentCapacity += 1;
            if (currentCapacity > capacity) {
                Node lruNode = removeLRUNode();
                hm.remove(lruNode.key);
                currentCapacity -= 1;
            }
        }
        else {
            Node node = hm.get(key);
            node.value = value;
            moveNodeToHead(node);
        }
    }
}
```
