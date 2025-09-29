```
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

class Heap {
public:
    vector<int> arr;
    
    void insertElements() {
        arr.clear();
        cout << "Enter elements (enter -1 to stop): ";
        int val;
        while (cin >> val && val != -1) {
            arr.push_back(val);
        }
        cout << "Elements inserted!" << endl;
    }
    
    void maxHeapifyDown(int i, int n) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        
        if (left < n && arr[left] > arr[largest])
            largest = left;
        if (right < n && arr[right] > arr[largest])
            largest = right;
            
        if (largest != i) {
            swap(arr[i], arr[largest]);
            maxHeapifyDown(largest, n);
        }
    }
    
    void minHeapifyDown(int i, int n) {
        int smallest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        
        if (left < n && arr[left] < arr[smallest])
            smallest = left;
        if (right < n && arr[right] < arr[smallest])
            smallest = right;
            
        if (smallest != i) {
            swap(arr[i], arr[smallest]);
            minHeapifyDown(smallest, n);
        }
    }
    
    void maxHeapifyUp(int i) {
        int parent = (i - 1) / 2;
        if (i > 0 && arr[i] > arr[parent]) {
            swap(arr[i], arr[parent]);
            maxHeapifyUp(parent);
        }
    }
    
    void minHeapifyUp(int i) {
        int parent = (i - 1) / 2;
        if (i > 0 && arr[i] < arr[parent]) {
            swap(arr[i], arr[parent]);
            minHeapifyUp(parent);
        }
    }
    
    void buildMaxHeap() {
        for (int i = arr.size() / 2 - 1; i >= 0; i--) {
            maxHeapifyDown(i, arr.size());
        }
        cout << "Max heap created!" << endl;
    }
    
    void buildMinHeap() {
        for (int i = arr.size() / 2 - 1; i >= 0; i--) {
            minHeapifyDown(i, arr.size());
        }
        cout << "Min heap created!" << endl;
    }
    
    void deleteValueFromMaxHeap() {
        int val;
        cout << "Enter value to delete: ";
        cin >> val;
        
        int idx = -1;
        for (int i = 0; i < arr.size(); i++) {
            if (arr[i] == val) {
                idx = i;
                break;
            }
        }
        
        if (idx == -1) {
            cout << "Value not found!" << endl;
            return;
        }
        
        arr[idx] = arr.back();
        arr.pop_back();
        
        if (idx < arr.size()) {
            maxHeapifyDown(idx, arr.size());
            maxHeapifyUp(idx);
        }
        
        cout << "Value deleted from max heap!" << endl;
    }
    
    void deleteValueFromMinHeap() {
        int val;
        cout << "Enter value to delete: ";
        cin >> val;
        
        int idx = -1;
        for (int i = 0; i < arr.size(); i++) {
            if (arr[i] == val) {
                idx = i;
                break;
            }
        }
        
        if (idx == -1) {
            cout << "Value not found!" << endl;
            return;
        }
        
        arr[idx] = arr.back();
        arr.pop_back();
        
        if (idx < arr.size()) {
            minHeapifyDown(idx, arr.size());
            minHeapifyUp(idx);
        }
        
        cout << "Value deleted from min heap!" << endl;
    }
    
    void displayHeap() {
        cout << "Heap: ";
        for (int val : arr) {
            cout << val << " ";
        }
        cout << endl;
    }
    
    void deleteMinFromMinHeap() {
        if (arr.empty()) {
            cout << "Heap is empty!" << endl;
            return;
        }
        
        arr[0] = arr.back();
        arr.pop_back();
        if (!arr.empty()) {
            minHeapifyDown(0, arr.size());
        }
        
        cout << "Minimum deleted from min heap!" << endl;
    }
    
    void deleteMaxFromMinHeap() {
        if (arr.empty()) {
            cout << "Heap is empty!" << endl;
            return;
        }
        
        int maxIdx = arr.size() / 2;
        for (int i = arr.size() / 2; i < arr.size(); i++) {
            if (arr[i] > arr[maxIdx]) {
                maxIdx = i;
            }
        }
        
        arr[maxIdx] = arr.back();
        arr.pop_back();
        if (maxIdx < arr.size()) {
            minHeapifyDown(maxIdx, arr.size());
            minHeapifyUp(maxIdx);
        }
        
        cout << "Maximum deleted from min heap!" << endl;
    }
    
    void deleteMinFromMaxHeap() {
        if (arr.empty()) {
            cout << "Heap is empty!" << endl;
            return;
        }
        
        int minIdx = arr.size() / 2;
        for (int i = arr.size() / 2; i < arr.size(); i++) {
            if (arr[i] < arr[minIdx]) {
                minIdx = i;
            }
        }
        
        arr[minIdx] = arr.back();
        arr.pop_back();
        if (minIdx < arr.size()) {
            maxHeapifyDown(minIdx, arr.size());
            maxHeapifyUp(minIdx);
        }
        
        cout << "Minimum deleted from max heap!" << endl;
    }
    
    void deleteMaxFromMaxHeap() {
        if (arr.empty()) {
            cout << "Heap is empty!" << endl;
            return;
        }
        
        arr[0] = arr.back();
        arr.pop_back();
        if (!arr.empty()) {
            maxHeapifyDown(0, arr.size());
        }
        
        cout << "Maximum deleted from max heap!" << endl;
    }
    
    void heapSort() {
        vector<int> temp = arr;
        buildMaxHeap();
        
        for (int i = arr.size() - 1; i > 0; i--) {
            swap(arr[0], arr[i]);
            maxHeapifyDown(0, i);
        }
        
        cout << "Heap sorted array: ";
        for (int val : arr) {
            cout << val << " ";
        }
        cout << endl;
        
        arr = temp;
    }
    
    void insertAtEnd() {
        int val;
        cout << "Enter value to insert: ";
        cin >> val;
        
        cout << "Before insertion: ";
        displayHeap();
        
        arr.push_back(val);
        minHeapifyUp(arr.size() - 1);
        
        cout << "After insertion: ";
        displayHeap();
    }
    
    Node* insertInTree(Node* root, int val) {
        if (root == nullptr) {
            return new Node(val);
        }
        if (val < root->data) {
            root->left = insertInTree(root->left, val);
        } else if (val > root->data) {
            root->right = insertInTree(root->right, val);
        }
        return root;
    }
    
    void treeToVector(Node* root, vector<int>& vec) {
        if (root == nullptr) return;
        
        queue<Node*> q;
        q.push(root);
        
        while (!q.empty()) {
            Node* curr = q.front();
            q.pop();
            
            vec.push_back(curr->data);
            
            if (curr->left) q.push(curr->left);
            if (curr->right) q.push(curr->right);
        }
    }
    
    void binaryTreeToMaxHeap() {
        Node* root = nullptr;
        cout << "Enter values for tree (enter -1 to stop): ";
        int val;
        while (cin >> val && val != -1) {
            root = insertInTree(root, val);
        }
        
        arr.clear();
        treeToVector(root, arr);
        buildMaxHeap();
        cout << "Binary tree converted to max heap!" << endl;
    }
    
    void binaryTreeToMinHeap() {
        Node* root = nullptr;
        cout << "Enter values for tree (enter -1 to stop): ";
        int val;
        while (cin >> val && val != -1) {
            root = insertInTree(root, val);
        }
        
        arr.clear();
        treeToVector(root, arr);
        buildMinHeap();
        cout << "Binary tree converted to min heap!" << endl;
    }
    
    bool compareStructure(Node* t1, Node* t2) {
        if (t1 == nullptr && t2 == nullptr) return true;
        if (t1 == nullptr || t2 == nullptr) return false;
        
        return compareStructure(t1->left, t2->left) && 
               compareStructure(t1->right, t2->right);
    }
    
    void compareTwoHeaps() {
        Node* root1 = nullptr;
        Node* root2 = nullptr;
        
        cout << "Enter values for heap1 (enter -1 to stop): ";
        int val;
        while (cin >> val && val != -1) {
            root1 = insertInTree(root1, val);
        }
        
        cout << "Enter values for heap2 (enter -1 to stop): ";
        while (cin >> val && val != -1) {
            root2 = insertInTree(root2, val);
        }
        
        if (compareStructure(root1, root2)) {
            cout << "Both heaps have same structure!" << endl;
        } else {
            cout << "Heaps have different structure!" << endl;
        }
    }
    
    void displayTree() {
        if (arr.empty()) {
            cout << "Heap is empty!" << endl;
            return;
        }
        
        int h = 0;
        int temp = arr.size();
        while (temp > 0) {
            temp /= 2;
            h++;
        }
        
        int idx = 0;
        for (int level = 0; level < h && idx < arr.size(); level++) {
            int nodesInLevel = 1 << level;
            for (int i = 0; i < nodesInLevel && idx < arr.size(); i++) {
                cout << arr[idx++] << " ";
            }
            cout << endl;
        }
    }
    
    int findHeight() {
        if (arr.empty()) return 0;
        
        int h = 0;
        int temp = arr.size();
        while (temp > 0) {
            temp /= 2;
            h++;
        }
        
        cout << "Height: " << h - 1 << endl;
        return h - 1;
    }
    
    void minHeapToMaxHeap() {
        buildMaxHeap();
        cout << "Converted min heap to max heap!" << endl;
    }
    
    void maxHeapToMinHeap() {
        buildMinHeap();
        cout << "Converted max heap to min heap!" << endl;
    }
    
    bool isMinHeap(Node* root, int idx, int n) {
        if (root == nullptr) return true;
        
        if (idx >= n) return false;
        
        if (root->left && root->data > root->left->data) return false;
        if (root->right && root->data > root->right->data) return false;
        
        return isMinHeap(root->left, 2 * idx + 1, n) && 
               isMinHeap(root->right, 2 * idx + 2, n);
    }
    
    int countNodes(Node* root) {
        if (root == nullptr) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
    
    void checkAndConvertToMinHeap() {
        Node* root = nullptr;
        cout << "Enter values for tree (enter -1 to stop): ";
        int val;
        while (cin >> val && val != -1) {
            root = insertInTree(root, val);
        }
        
        int n = countNodes(root);
        
        if (isMinHeap(root, 0, n)) {
            cout << "Tree is already a min heap!" << endl;
        } else {
            cout << "Tree is not a min heap, converting..." << endl;
            arr.clear();
            treeToVector(root, arr);
            buildMinHeap();
            cout << "Converted to min heap!" << endl;
        }
    }
    
    bool isMaxHeap(Node* root, int idx, int n) {
        if (root == nullptr) return true;
        
        if (idx >= n) return false;
        
        if (root->left && root->data < root->left->data) return false;
        if (root->right && root->data < root->right->data) return false;
        
        return isMaxHeap(root->left, 2 * idx + 1, n) && 
               isMaxHeap(root->right, 2 * idx + 2, n);
    }
    
    void checkAndConvertToMaxHeap() {
        Node* root = nullptr;
        cout << "Enter values for tree (enter -1 to stop): ";
        int val;
        while (cin >> val && val != -1) {
            root = insertInTree(root, val);
        }
        
        int n = countNodes(root);
        
        if (isMaxHeap(root, 0, n)) {
            cout << "Tree is already a max heap!" << endl;
        } else {
            cout << "Tree is not a max heap, converting..." << endl;
            arr.clear();
            treeToVector(root, arr);
            buildMaxHeap();
            cout << "Converted to max heap!" << endl;
        }
    }
};

int main() {
    Heap h;
    int choice;
    
    while (true) {
        cout << "\n=== HEAP MENU ===" << endl;
        cout << "1. Insert elements" << endl;
        cout << "2. Create max heap" << endl;
        cout << "3. Create min heap" << endl;
        cout << "4. Delete value from max heap" << endl;
        cout << "5. Delete value from min heap" << endl;
        cout << "6. Display heap" << endl;
        cout << "7. Delete min from min heap" << endl;
        cout << "8. Delete max from min heap" << endl;
        cout << "9. Delete min from max heap" << endl;
        cout << "10. Delete max from max heap" << endl;
        cout << "11. Heap sort" << endl;
        cout << "12. Insert at end" << endl;
        cout << "13. Binary tree to max heap" << endl;
        cout << "14. Binary tree to min heap" << endl;
        cout << "15. Compare two heaps structure" << endl;
        cout << "16. Display tree levels" << endl;
        cout << "17. Find height" << endl;
        cout << "18. Min heap to max heap" << endl;
        cout << "19. Max heap to min heap" << endl;
        cout << "20. Check and convert to min heap" << endl;
        cout << "21. Check and convert to max heap" << endl;
        cout << "0. Exit" << endl;
        cout << "Choice: ";
        cin >> choice;
        
        switch (choice) {
            case 1: h.insertElements(); break;
            case 2: h.buildMaxHeap(); break;
            case 3: h.buildMinHeap(); break;
            case 4: h.deleteValueFromMaxHeap(); break;
            case 5: h.deleteValueFromMinHeap(); break;
            case 6: h.displayHeap(); break;
            case 7: h.deleteMinFromMinHeap(); break;
            case 8: h.deleteMaxFromMinHeap(); break;
            case 9: h.deleteMinFromMaxHeap(); break;
            case 10: h.deleteMaxFromMaxHeap(); break;
            case 11: h.heapSort(); break;
            case 12: h.insertAtEnd(); break;
            case 13: h.binaryTreeToMaxHeap(); break;
            case 14: h.binaryTreeToMinHeap(); break;
            case 15: h.compareTwoHeaps(); break;
            case 16: h.displayTree(); break;
            case 17: h.findHeight(); break;
            case 18: h.minHeapToMaxHeap(); break;
            case 19: h.maxHeapToMinHeap(); break;
            case 20: h.checkAndConvertToMinHeap(); break;
            case 21: h.checkAndConvertToMaxHeap(); break;
            case 0: return 0;
            default: cout << "Invalid choice!" << endl;
        }
    }
    
    return 0;
}
```


```
# Heap Functions Explained for Complete Noobs

## insertElements()
Clears the vector, asks you to type numbers, keeps adding them until you type -1. Basic input function fr.

## maxHeapifyDown(i, n)
Takes an index `i` and size `n`. Checks if parent is bigger than both children. If not, swaps parent with the LARGEST child and keeps going down recursively. This maintains **max heap property** (parent > children).

Example: parent=5, left=10, right=7 → swap 5 and 10, now check position where 10 was

## minHeapifyDown(i, n)
Same vibe as maxHeapifyDown but swaps with SMALLEST child. Maintains **min heap property** (parent < children).

## maxHeapifyUp(i)
Opposite direction - goes UP from child to parent. If child is bigger than parent, swap and keep going up. Used after inserting at the end.

Example: you insert 100 at end, it bubbles up until it finds its correct spot

## minHeapifyUp(i)
Same but child must be smaller than parent to swap. Bubbles small values to the top.

## buildMaxHeap()
Converts your random vector into a max heap. Starts from the last non-leaf node (size/2 - 1) and calls maxHeapifyDown on everything going backwards.

Why backwards? Cause you gotta fix children before parents, bottom-up approach is goated.

**Key concept: non-leaf nodes** start at index `size/2 - 1` because in array representation, anything after that is a leaf (no children).

## buildMinHeap()
Same as buildMaxHeap but makes min heap instead.

## deleteValueFromMaxHeap()
Searches for your value in the vector (O(n) search, kinda mid performance ngl). Once found:
1. Replace it with LAST element
2. Remove last element (pop_back)
3. Heapify both UP and DOWN at that position

Why both? Cause idk if the replacement value is too big or too small, so check both directions.

## deleteValueFromMinHeap()
Same logic as deleteValueFromMaxHeap but for min heap.

## displayHeap()
Just prints the vector as an array. Shows the heap in **level-order** (root first, then its children left to right, etc).

## deleteMinFromMinHeap()
Root is the minimum in min heap. Replace root with last element, yeet the last, then heapifyDown from root. Classic root deletion.

## deleteMaxFromMinHeap()
In a min heap, the MAX value is always in the **leaves** (nodes with no children). Leaves start at index `size/2`. Loop through all leaves, find the biggest one, then delete it like deleteValueFromMinHeap.

**Why leaves start at size/2?** Array-based heap property - any index >= size/2 has no children.

## deleteMinFromMaxHeap()
Opposite - in max heap, MIN is in the leaves. Same logic as deleteMaxFromMinHeap but find the smallest leaf.

## deleteMaxFromMaxHeap()
Root is maximum. Same as deleteMinFromMinHeap logic.

## heapSort()
1. Saves current vector to temp
2. Builds max heap
3. Repeatedly swaps root (max) with last element, reduces heap size by 1, heapifies remaining part
4. Result = sorted array
5. Restores original vector from temp

Basically extracting max repeatedly gives you sorted order (descending first, but ends up ascending in the array).

## insertAtEnd()
Adds new value to end of vector, then calls minHeapifyUp to bubble it to correct position.

⚠️ **BUG ALERT:** Assumes min heap, breaks if you built a max heap. This function is lowkey broken for max heaps.

## insertInTree(root, val)
Classic **BST insertion** (Binary Search Tree, not heap). If val < root, go left. If val > root, go right. Finds empty spot and inserts.

**BST property:** left child < parent < right child (different from heap!)

## treeToVector(root, vec)
Does **level-order traversal** using a queue. Visits tree level by level (root, then all children, then all grandchildren, etc) and dumps values into vector.

**Level-order traversal** = BFS (Breadth First Search) - uses a queue, processes nodes level by level.

## binaryTreeToMaxHeap()
1. Builds BST from your input
2. Converts BST to vector using level-order
3. Calls buildMaxHeap to heapify it

Converts any tree structure to max heap.

## binaryTreeToMinHeap()
Same as above but makes min heap.

## compareStructure(t1, t2)
Recursively checks if two trees have same SHAPE (ignores values). Both must have children in same positions. If t1 has left child, t2 must too.

## compareTwoHeaps()
Builds two trees from input, calls compareStructure to check if they're shaped the same.

## displayTree()
Prints heap level by level (each level on new line). Calculates height first, then prints nodes level by level using array indices.

## findHeight()
Counts how many times you can divide size by 2 until you hit 0. That's the height.

Example: size=7 → 7/2=3, 3/2=1, 1/2=0 → 3 divisions → height = 2 (height is divisions - 1)

**Height** = number of edges from root to deepest leaf.

## minHeapToMaxHeap()
Just calls buildMaxHeap on current vector. Rearranges values to satisfy max heap property.

## maxHeapToMinHeap()
Same but calls buildMinHeap.

## isMinHeap(root, idx, n)
Recursively checks if tree satisfies min heap property:
- Parent must be <= both children
- Checks this for every node

Returns true if valid min heap, false otherwise.

## countNodes(root)
Recursively counts all nodes in tree. 1 (current) + left subtree count + right subtree count.

## checkAndConvertToMinHeap()
Builds tree from input, counts nodes, checks if it's already a min heap. If not, converts to vector and builds min heap.

## isMaxHeap(root, idx, n)
Same as isMinHeap but checks parent >= children.

## checkAndConvertToMaxHeap()
Same as checkAndConvertToMinHeap but for max heap.

---

# KEY CONCEPTS YOU SHOULD KNOW

- **Complete binary tree:** All levels filled except maybe last, fills left to right
- **Heap property:** Parent-child relationship (max: parent > child, min: parent < child)
- **Heapify up:** Bubble value UP the tree (child to parent)
- **Heapify down:** Push value DOWN the tree (parent to children)
- **Array indexing:** For index i, left child = 2i+1, right child = 2i+2, parent = (i-1)/2
- **Leaves start at size/2:** Everything after that has no children
- **Level-order traversal:** Visit nodes level by level using a queue
```
```
# C++ Vector Cheatsheet

## Basics
```cpp
#include <vector>
#include <algorithm>  // for sort, reverse, etc

vector<int> v;                    // empty vector
vector<int> v(5);                 // size 5, all zeros
vector<int> v(5, 10);             // size 5, all 10s
vector<int> v = {1, 2, 3, 4};     // initialize with values
```

## Insert/Add Elements
```cpp
v.push_back(10);                  // add 10 at end
v.insert(v.begin(), 5);           // insert 5 at front
v.insert(v.begin() + 2, 7);       // insert 7 at index 2
v.insert(v.end(), 3, 100);        // insert three 100s at end
```

## Delete/Remove Elements
```cpp
v.pop_back();                     // remove last element
v.erase(v.begin());               // remove first element
v.erase(v.begin() + 2);           // remove element at index 2
v.erase(v.begin() + 1, v.begin() + 4);  // remove range [1,4)
v.clear();                        // remove all elements
```

## Access Elements
```cpp
v[2];                             // access index 2 (no bounds check)
v.at(2);                          // access index 2 (throws error if out of bounds)
v.front();                        // first element
v.back();                         // last element
```

## Size Operations
```cpp
v.size();                         // number of elements
v.empty();                        // returns true if empty
v.resize(10);                     // resize to 10 elements
v.resize(10, 5);                  // resize to 10, fill new with 5
```

## Sort Operations
```cpp
sort(v.begin(), v.end());         // ascending sort
sort(v.begin(), v.end(), greater<int>());  // descending sort
reverse(v.begin(), v.end());      // reverse the vector
```

## Combine Two Vectors
```cpp
vector<int> v1 = {1, 2, 3};
vector<int> v2 = {4, 5, 6};

// Method 1: insert
v1.insert(v1.end(), v2.begin(), v2.end());

// Method 2: loop
for(int x : v2) {
    v1.push_back(x);
}
```

## Find/Search
```cpp
auto it = find(v.begin(), v.end(), 10);   // find value 10
if (it != v.end()) {
    int index = it - v.begin();           // get index
}

// count occurrences
int cnt = count(v.begin(), v.end(), 10);

// check if value exists
bool exists = (find(v.begin(), v.end(), 10) != v.end());
```

## Swap
```cpp
swap(v[0], v[2]);                 // swap elements at index 0 and 2
v1.swap(v2);                      // swap entire vectors
```

## Min/Max
```cpp
int minVal = *min_element(v.begin(), v.end());
int maxVal = *max_element(v.begin(), v.end());

// get iterator to min/max
auto minIt = min_element(v.begin(), v.end());
auto maxIt = max_element(v.begin(), v.end());
int minIndex = minIt - v.begin();
```

## Loop Through Vector
```cpp
// Method 1: range-based for
for(int x : v) {
    cout << x << " ";
}

// Method 2: iterator
for(auto it = v.begin(); it != v.end(); it++) {
    cout << *it << " ";
}

// Method 3: index
for(int i = 0; i < v.size(); i++) {
    cout << v[i] << " ";
}
```

## Copy Vector
```cpp
vector<int> v2 = v1;              // copy v1 to v2
vector<int> v2(v1.begin(), v1.end());  // another way
```

## Remove Duplicates
```cpp
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());
```

## 2D Vector
```cpp
vector<vector<int>> grid(3, vector<int>(4, 0));  // 3x4 grid of zeros
grid[1][2] = 5;                   // access row 1, col 2
```

## Common Pitfalls
```cpp
// DON'T do this - out of bounds
vector<int> v(5);
v[10] = 100;  // undefined behavior

// DO this instead
v.resize(11);
v[10] = 100;

// or use push_back
v.push_back(100);
```
```
