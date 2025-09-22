// CODE

#include <iostream>
#include <vector>
#include <set>
#include <climits>
#include <cmath>
#include <algorithm>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    
    Node(int value) : data(value), left(nullptr), right(nullptr) {}
};

class BST {
private:
    Node* root;
    
    Node* insertRec(Node* node, int data) {
        if (node == nullptr) {
            return new Node(data);
        }
        
        if (data < node->data) {
            node->left = insertRec(node->left, data);
        } else if (data > node->data) {
            node->right = insertRec(node->right, data);
        }
        
        return node;
    }
    
    Node* deleteRec(Node* node, int data) {
        if (node == nullptr) return node;
        
        if (data < node->data) {
            node->left = deleteRec(node->left, data);
        } else if (data > node->data) {
            node->right = deleteRec(node->right, data);
        } else {
            if (node->left == nullptr) {
                Node* temp = node->right;
                delete node;
                return temp;
            } else if (node->right == nullptr) {
                Node* temp = node->left;
                delete node;
                return temp;
            }
            
            Node* temp = findMin(node->right);
            node->data = temp->data;
            node->right = deleteRec(node->right, temp->data);
        }
        return node;
    }
    
    void preorderRec(Node* node) {
        if (node != nullptr) {
            cout << node->data << " ";
            preorderRec(node->left);
            preorderRec(node->right);
        }
    }
    
    void postorderRec(Node* node) {
        if (node != nullptr) {
            postorderRec(node->left);
            postorderRec(node->right);
            cout << node->data << " ";
        }
    }
    
    void inorderRec(Node* node) {
        if (node != nullptr) {
            inorderRec(node->left);
            cout << node->data << " ";
            inorderRec(node->right);
        }
    }
    
    Node* findMin(Node* node) {
        while (node && node->left != nullptr) {
            node = node->left;
        }
        return node;
    }
    
    Node* findMax(Node* node) {
        while (node && node->right != nullptr) {
            node = node->right;
        }
        return node;
    }
    
    int getHeight(Node* node) {
        if (node == nullptr) return -1;
        return 1 + max(getHeight(node->left), getHeight(node->right));
    }
    
    int getDepth(Node* node, int value, int depth) {
        if (node == nullptr) return -1;
        if (node->data == value) return depth;
        
        int leftDepth = getDepth(node->left, value, depth + 1);
        if (leftDepth != -1) return leftDepth;
        
        return getDepth(node->right, value, depth + 1);
    }
    
    Node* findNode(Node* node, int value) {
        if (node == nullptr || node->data == value) {
            return node;
        }
        
        if (value < node->data) {
            return findNode(node->left, value);
        }
        return findNode(node->right, value);
    }
    
    Node* findParent(Node* node, int value) {
        if (node == nullptr || node->data == value) {
            return nullptr;
        }
        
        if ((node->left && node->left->data == value) || 
            (node->right && node->right->data == value)) {
            return node;
        }
        
        if (value < node->data) {
            return findParent(node->left, value);
        }
        return findParent(node->right, value);
    }
    
    bool isBSTRec(Node* node, int minVal, int maxVal) {
        if (node == nullptr) return true;
        
        if (node->data <= minVal || node->data >= maxVal) {
            return false;
        }
        
        return isBSTRec(node->left, minVal, node->data) && 
               isBSTRec(node->right, node->data, maxVal);
    }
    
    void countInRange(Node* node, int low, int high, int& count) {
        if (node == nullptr) return;
        
        if (node->data >= low && node->data <= high) {
            count++;
        }
        
        if (node->data > low) {
            countInRange(node->left, low, high, count);
        }
        
        if (node->data < high) {
            countInRange(node->right, low, high, count);
        }
    }
    
    void inorderVector(Node* node, vector<int>& result) {
        if (node != nullptr) {
            inorderVector(node->left, result);
            result.push_back(node->data);
            inorderVector(node->right, result);
        }
    }
    
    Node* findLCA(Node* node, int n1, int n2) {
        if (node == nullptr) return nullptr;
        
        if (node->data > n1 && node->data > n2) {
            return findLCA(node->left, n1, n2);
        }
        
        if (node->data < n1 && node->data < n2) {
            return findLCA(node->right, n1, n2);
        }
        
        return node;
    }
    
    Node* findPredecessor(Node* node, int value) {
        Node* target = findNode(node, value);
        if (target == nullptr) return nullptr;
        
        if (target->left != nullptr) {
            return findMax(target->left);
        }
        
        Node* predecessor = nullptr;
        while (node != nullptr) {
            if (value > node->data) {
                predecessor = node;
                node = node->right;
            } else if (value < node->data) {
                node = node->left;
            } else {
                break;
            }
        }
        return predecessor;
    }
    
    Node* findSuccessor(Node* node, int value) {
        Node* target = findNode(node, value);
        if (target == nullptr) return nullptr;
        
        if (target->right != nullptr) {
            return findMin(target->right);
        }
        
        Node* successor = nullptr;
        while (node != nullptr) {
            if (value < node->data) {
                successor = node;
                node = node->left;
            } else if (value > node->data) {
                node = node->right;
            } else {
                break;
            }
        }
        return successor;
    }
    
    int countNodes(Node* node) {
        if (node == nullptr) return 0;
        return 1 + countNodes(node->left) + countNodes(node->right);
    }

public:
    BST() : root(nullptr) {}
    
    void insert(int data) {
        root = insertRec(root, data);
    }
    
    void deleteNode(int data) {
        root = deleteRec(root, data);
    }
    
    void preorder() {
        cout << "Preorder: ";
        preorderRec(root);
        cout << endl;
    }
    
    void postorder() {
        cout << "Postorder: ";
        postorderRec(root);
        cout << endl;
    }
    
    void inorder() {
        cout << "Inorder: ";
        inorderRec(root);
        cout << endl;
    }
    
    void findMaxValue() {
        if (root == nullptr) {
            cout << "Tree is empty" << endl;
            return;
        }
        Node* maxNode = findMax(root);
        cout << "Maximum value: " << maxNode->data << endl;
    }
    
    void findMinValue() {
        if (root == nullptr) {
            cout << "Tree is empty" << endl;
            return;
        }
        Node* minNode = findMin(root);
        cout << "Minimum value: " << minNode->data << endl;
    }
    
    void getNodeDepthHeight() {
        int value;
        cout << "Enter node value: ";
        cin >> value;
        
        Node* node = findNode(root, value);
        if (node == nullptr) {
            cout << "Node not found" << endl;
            return;
        }
        
        int depth = getDepth(root, value, 0);
        int height = getHeight(node);
        
        cout << "Node " << value << " - Depth: " << depth << ", Height: " << height << endl;
    }
    
    void calculateLevels() {
        int levels = getHeight(root) + 1;
        cout << "Number of levels in BST: " << levels << endl;
    }
    
    void findChildNodes() {
        int value;
        cout << "Enter node value: ";
        cin >> value;
        
        Node* node = findNode(root, value);
        if (node == nullptr) {
            cout << "Node not found" << endl;
            return;
        }
        
        cout << "Children of node " << value << ": ";
        if (node->left) cout << "Left: " << node->left->data << " ";
        if (node->right) cout << "Right: " << node->right->data << " ";
        if (!node->left && !node->right) cout << "No children";
        cout << endl;
    }
    
    void findParentNode() {
        int value;
        cout << "Enter node value: ";
        cin >> value;
        
        Node* parent = findParent(root, value);
        if (parent == nullptr) {
            if (root && root->data == value) {
                cout << "Node " << value << " is the root (no parent)" << endl;
            } else {
                cout << "Node not found" << endl;
            }
        } else {
            cout << "Parent of node " << value << ": " << parent->data << endl;
        }
    }
    
    void leftRightmost() {
        int value;
        cout << "Enter node value: ";
        cin >> value;
        
        Node* node = findNode(root, value);
        if (node == nullptr || node->left == nullptr) {
            cout << "Node not found or has no left subtree" << endl;
            return;
        }
        
        Node* rightmost = findMax(node->left);
        cout << "Left subtree's rightmost node of " << value << ": " << rightmost->data << endl;
    }
    
    void rightLeftmost() {
        int value;
        cout << "Enter node value: ";
        cin >> value;
        
        Node* node = findNode(root, value);
        if (node == nullptr || node->right == nullptr) {
            cout << "Node not found or has no right subtree" << endl;
            return;
        }
        
        Node* leftmost = findMin(node->right);
        cout << "Right subtree's leftmost node of " << value << ": " << leftmost->data << endl;
    }
    
    void leftLeftmost() {
        int value;
        cout << "Enter node value: ";
        cin >> value;
        
        Node* node = findNode(root, value);
        if (node == nullptr || node->left == nullptr) {
            cout << "Node not found or has no left subtree" << endl;
            return;
        }
        
        Node* leftmost = findMin(node->left);
        cout << "Left subtree's leftmost node of " << value << ": " << leftmost->data << endl;
    }
    
    void rightRightmost() {
        int value;
        cout << "Enter node value: ";
        cin >> value;
        
        Node* node = findNode(root, value);
        if (node == nullptr || node->right == nullptr) {
            cout << "Node not found or has no right subtree" << endl;
            return;
        }
        
        Node* rightmost = findMax(node->right);
        cout << "Right subtree's rightmost node of " << value << ": " << rightmost->data << endl;
    }
    
    void checkIsBST() {
        bool result = isBSTRec(root, INT_MIN, INT_MAX);
        cout << "Is the tree a valid BST? " << (result ? "Yes" : "No") << endl;
    }
    
    void countNodesInRange() {
        int low, high;
        cout << "Enter lower bound: ";
        cin >> low;
        cout << "Enter higher bound: ";
        cin >> high;
        
        int count = 0;
        countInRange(root, low, high, count);
        cout << "Number of nodes in range [" << low << ", " << high << "]: " << count << endl;
    }
    
    void calculateMinMaxLevels() {
        int totalNodes;
        cout << "Enter total number of nodes: ";
        cin >> totalNodes;
        
        if (totalNodes <= 0) {
            cout << "Invalid number of nodes" << endl;
            return;
        }
        
        int minLevels = (int)ceil(log2(totalNodes + 1));
        int maxLevels = totalNodes;
        
        cout << "For " << totalNodes << " nodes:" << endl;
        cout << "Minimum levels (balanced tree): " << minLevels << endl;
        cout << "Maximum levels (skewed tree): " << maxLevels << endl;
    }
    
    void searchNode() {
        int value;
        cout << "Enter value to search: ";
        cin >> value;
        
        Node* node = findNode(root, value);
        if (node != nullptr) {
            cout << "Node " << value << " is present in the BST" << endl;
        } else {
            cout << "Node " << value << " is not present in the BST" << endl;
        }
    }
    
    void findLeastCommonAncestor() {
        int n1, n2;
        cout << "Enter first node value: ";
        cin >> n1;
        cout << "Enter second node value: ";
        cin >> n2;
        
        Node* lca = findLCA(root, n1, n2);
        if (lca != nullptr) {
            cout << "Least Common Ancestor of " << n1 << " and " << n2 << ": " << lca->data << endl;
        } else {
            cout << "LCA not found (one or both nodes may not exist)" << endl;
        }
    }
    
    void findMedian() {
        vector<int> values;
        inorderVector(root, values);
        
        if (values.empty()) {
            cout << "Tree is empty" << endl;
            return;
        }
        
        int size = values.size();
        if (size % 2 == 1) {
            cout << "Median: " << values[size / 2] << endl;
        } else {
            double median = (values[size / 2 - 1] + values[size / 2]) / 2.0;
            cout << "Median: " << median << endl;
        }
    }
    
    void findUniqueValues() {
        cout << "Note: BST by definition contains only unique values" << endl;
        cout << "All values in BST (inorder): ";
        inorder();
    }
    
    void findPairWithSum() {
        int targetSum;
        cout << "Enter target sum: ";
        cin >> targetSum;
        
        vector<int> values;
        inorderVector(root, values);
        
        bool found = false;
        for (int i = 0; i < values.size() - 1; i++) {
            for (int j = i + 1; j < values.size(); j++) {
                if (values[i] + values[j] == targetSum) {
                    cout << "Pair found: " << values[i] << " + " << values[j] << " = " << targetSum << endl;
                    found = true;
                }
            }
        }
        
        if (!found) {
            cout << "No such pair found" << endl;
        }
    }
    
    void findPredecessorSuccessor() {
        int value;
        cout << "Enter node value: ";
        cin >> value;
        
        Node* pred = findPredecessor(root, value);
        Node* succ = findSuccessor(root, value);
        
        cout << "For node " << value << ":" << endl;
        if (pred) {
            cout << "Predecessor: " << pred->data << endl;
        } else {
            cout << "No predecessor" << endl;
        }
        
        if (succ) {
            cout << "Successor: " << succ->data << endl;
        } else {
            cout << "No successor" << endl;
        }
    }
    
    void findMaxHeightDepth() {
        if (root == nullptr) {
            cout << "Tree is empty" << endl;
            return;
        }
        
        int maxHeight = getHeight(root);
        int maxDepth = maxHeight; // In a tree, max depth = height of root
        
        cout << "Maximum height of BST: " << maxHeight << endl;
        cout << "Maximum depth of BST: " << maxDepth << endl;
    }
};

void displayMenu() {
    cout << "\n=== BST Operations Menu ===" << endl;
    cout << "1) Insert data" << endl;
    cout << "2) Delete data by value" << endl;
    cout << "3) Preorder traversal" << endl;
    cout << "4) Postorder traversal" << endl;
    cout << "5) Inorder traversal" << endl;
    cout << "6) Find max value in BST" << endl;
    cout << "7) Find min value in BST" << endl;
    cout << "8) Get depth and height of a node" << endl;
    cout << "9) Calculate levels of BST" << endl;
    cout << "10) Find child nodes of a node" << endl;
    cout << "11) Find parent node of a node" << endl;
    cout << "12) Left's rightmost node of given node" << endl;
    cout << "13) Right's leftmost node of given node" << endl;
    cout << "14) Left's leftmost node of given node" << endl;
    cout << "15) Right's rightmost node of given node" << endl;
    cout << "16) Check if tree is BST" << endl;
    cout << "17) Count nodes in range" << endl;
    cout << "18) Calculate min/max levels based on node count" << endl;
    cout << "19) Search a node by value" << endl;
    cout << "20) Find least common ancestor of two nodes" << endl;
    cout << "21) Find median of BST" << endl;
    cout << "22) Find unique values in BST" << endl;
    cout << "23) Find pair which equals to sum" << endl;
    cout << "24) Find predecessor and successor of a node" << endl;
    cout << "25) Find max height and depth of BST" << endl;
    cout << "0) Exit" << endl;
    cout << "Enter your choice: ";
}

int main() {
    BST bst;
    int choice, value;
    
    while (true) {
        displayMenu();
        cin >> choice;
        
        switch (choice) {
            case 0:
                cout << "Exiting program. Goodbye!" << endl;
                return 0;
                
            case 1:
                cout << "Enter value to insert: ";
                cin >> value;
                bst.insert(value);
                cout << "Value inserted successfully" << endl;
                break;
                
            case 2:
                cout << "Enter value to delete: ";
                cin >> value;
                bst.deleteNode(value);
                cout << "Value deleted successfully" << endl;
                break;
                
            case 3:
                bst.preorder();
                break;
                
            case 4:
                bst.postorder();
                break;
                
            case 5:
                bst.inorder();
                break;
                
            case 6:
                bst.findMaxValue();
                break;
                
            case 7:
                bst.findMinValue();
                break;
                
            case 8:
                bst.getNodeDepthHeight();
                break;
                
            case 9:
                bst.calculateLevels();
                break;
                
            case 10:
                bst.findChildNodes();
                break;
                
            case 11:
                bst.findParentNode();
                break;
                
            case 12:
                bst.leftRightmost();
                break;
                
            case 13:
                bst.rightLeftmost();
                break;
                
            case 14:
                bst.leftLeftmost();
                break;
                
            case 15:
                bst.rightRightmost();
                break;
                
            case 16:
                bst.checkIsBST();
                break;
                
            case 17:
                bst.countNodesInRange();
                break;
                
            case 18:
                bst.calculateMinMaxLevels();
                break;
                
            case 19:
                bst.searchNode();
                break;
                
            case 20:
                bst.findLeastCommonAncestor();
                break;
                
            case 21:
                bst.findMedian();
                break;
                
            case 22:
                bst.findUniqueValues();
                break;
                
            case 23:
                bst.findPairWithSum();
                break;
                
            case 24:
                bst.findPredecessorSuccessor();
                break;
                
            case 25:
                bst.findMaxHeightDepth();
                break;
                
            default:
                cout << "Invalid choice. Please try again." << endl;
                break;
        }
    }
    
    return 0;
}




// two trees

#include <iostream>
#include <vector>
#include <set>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    
    Node(int val) {
        data = val;
        left = nullptr;
        right = nullptr;
    }
};

// Insert node in BST
Node* insert(Node* root, int val) {
    if (root == nullptr) {
        return new Node(val);
    }
    if (val < root->data) {
        root->left = insert(root->left, val);
    } else if (val > root->data) {
        root->right = insert(root->right, val);
    }
    return root;
}

// 1) Find common values in 2 trees
void getInorder(Node* root, set<int>& values) {
    if (root == nullptr) return;
    getInorder(root->left, values);
    values.insert(root->data);
    getInorder(root->right, values);
}

void findCommonValues(Node* tree1, Node* tree2) {
    set<int> values1, values2;
    getInorder(tree1, values1);
    getInorder(tree2, values2);
    
    cout << "Common values: ";
    for (int val : values1) {
        if (values2.count(val)) {
            cout << val << " ";
        }
    }
    cout << endl;
}

// 2) Check if structure is same
bool sameStructure(Node* tree1, Node* tree2) {
    if (tree1 == nullptr && tree2 == nullptr) return true;
    if (tree1 == nullptr || tree2 == nullptr) return false;
    
    return sameStructure(tree1->left, tree2->left) && 
           sameStructure(tree1->right, tree2->right);
}

// 3) Clone BST
Node* cloneBST(Node* root) {
    if (root == nullptr) {
        return nullptr;
    }
    Node* newNode = new Node(root->data);
    newNode->left = cloneBST(root->left);
    newNode->right = cloneBST(root->right);
    return newNode;
}

// 4) Traversal conversions
int search(vector<int>& arr, int x) {
    for (int i = 0; i < arr.size(); i++)
        if (arr[i] == x)
            return i;
    return -1;
}

void printPostOrder(vector<int>& in, vector<int>& pre,
                    int inStart, int inEnd, int preStart) {
    if (inStart > inEnd) return;
    
    int rootIndex = search(in, pre[preStart]);
    
    if (rootIndex > inStart) {
        printPostOrder(in, pre, inStart, rootIndex - 1, preStart + 1);
    }
    
    if (rootIndex < inEnd) {
        printPostOrder(in, pre, rootIndex + 1, 
                       inEnd, preStart + rootIndex - inStart + 1);
    }
    
    cout << pre[preStart] << " ";
}

void printPreOrder(vector<int>& in, vector<int>& post,
                   int inStart, int inEnd, int postEnd) {
    if (inStart > inEnd) return;
    
    int rootIndex = search(in, post[postEnd]);
    
    cout << post[postEnd] << " ";
    
    if (rootIndex > inStart) {
        printPreOrder(in, post, inStart, rootIndex - 1, 
                     postEnd - (inEnd - rootIndex) - 1);
    }
    
    if (rootIndex < inEnd) {
        printPreOrder(in, post, rootIndex + 1, inEnd, postEnd - 1);
    }
}

void printInorder(Node* root) {
    if (root == nullptr) return;
    printInorder(root->left);
    cout << root->data << " ";
    printInorder(root->right);
}

void displayMenu() {
    cout << "\n=== BST MENU SYSTEM ===" << endl;
    cout << "1. Find common values in 2 trees" << endl;
    cout << "2. Check if structure is same" << endl;
    cout << "3. Clone tree" << endl;
    cout << "4. Traversal conversions" << endl;
    cout << "5. Display tree (inorder)" << endl;
    cout << "6. Insert values to tree" << endl;
    cout << "0. Exit" << endl;
    cout << "Choice: ";
}

int main() {
    Node* tree1 = nullptr;
    Node* tree2 = nullptr;
    Node* clonedTree = nullptr;
    int choice;
    
    do {
        displayMenu();
        cin >> choice;
        
        switch(choice) {
            case 1: {
                if (tree1 == nullptr || tree2 == nullptr) {
                    cout << "Both trees need values first!" << endl;
                    break;
                }
                findCommonValues(tree1, tree2);
                break;
            }
            
            case 2: {
                if (tree1 == nullptr || tree2 == nullptr) {
                    cout << "Both trees need values first!" << endl;
                    break;
                }
                if (sameStructure(tree1, tree2)) {
                    cout << "Trees have same structure" << endl;
                } else {
                    cout << "Trees have different structure" << endl;
                }
                break;
            }
            
            case 3: {
                cout << "1. Clone tree1" << endl;
                cout << "2. Clone tree2" << endl;
                int cloneChoice;
                cin >> cloneChoice;
                
                if (cloneChoice == 1 && tree1 != nullptr) {
                    clonedTree = cloneBST(tree1);
                    cout << "Tree1 cloned successfully!" << endl;
                } else if (cloneChoice == 2 && tree2 != nullptr) {
                    clonedTree = cloneBST(tree2);
                    cout << "Tree2 cloned successfully!" << endl;
                } else {
                    cout << "Invalid choice or empty tree!" << endl;
                }
                break;
            }
            
            case 4: {
                cout << "1. Pre+In -> Post" << endl;
                cout << "2. Post+In -> Pre" << endl;
                int convChoice;
                cin >> convChoice;
                
                if (convChoice == 1) {
                    int n;
                    cout << "Enter number of nodes: ";
                    cin >> n;
                    
                    vector<int> preorder(n), inorder(n);
                    cout << "Enter preorder: ";
                    for (int i = 0; i < n; i++) cin >> preorder[i];
                    cout << "Enter inorder: ";
                    for (int i = 0; i < n; i++) cin >> inorder[i];
                    
                    cout << "Postorder: ";
                    printPostOrder(inorder, preorder, 0, n-1, 0);
                    cout << endl;
                } else if (convChoice == 2) {
                    int n;
                    cout << "Enter number of nodes: ";
                    cin >> n;
                    
                    vector<int> postorder(n), inorder(n);
                    cout << "Enter postorder: ";
                    for (int i = 0; i < n; i++) cin >> postorder[i];
                    cout << "Enter inorder: ";
                    for (int i = 0; i < n; i++) cin >> inorder[i];
                    
                    cout << "Preorder: ";
                    printPreOrder(inorder, postorder, 0, n-1, n-1);
                    cout << endl;
                }
                break;
            }
            
            case 5: {
                cout << "1. Tree1" << endl;
                cout << "2. Tree2" << endl;
                cout << "3. Cloned tree" << endl;
                int displayChoice;
                cin >> displayChoice;
                
                if (displayChoice == 1) {
                    cout << "Tree1 inorder: ";
                    printInorder(tree1);
                    cout << endl;
                } else if (displayChoice == 2) {
                    cout << "Tree2 inorder: ";
                    printInorder(tree2);
                    cout << endl;
                } else if (displayChoice == 3) {
                    cout << "Cloned tree inorder: ";
                    printInorder(clonedTree);
                    cout << endl;
                }
                break;
            }
            
            case 6: {
                cout << "1. Insert to Tree1" << endl;
                cout << "2. Insert to Tree2" << endl;
                int insertChoice;
                cin >> insertChoice;
                
                cout << "Enter values (enter -1 to stop): ";
                int val;
                while (cin >> val && val != -1) {
                    if (insertChoice == 1) {
                        tree1 = insert(tree1, val);
                    } else if (insertChoice == 2) {
                        tree2 = insert(tree2, val);
                    }
                }
                cout << "Values inserted!" << endl;
                break;
            }
            
            case 0:
                cout << "Exiting..." << endl;
                break;
                
            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice != 0);
    
    return 0;
}
