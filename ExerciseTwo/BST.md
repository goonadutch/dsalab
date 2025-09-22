# CODE

```
#include <iostream>
#include <cmath>
#include <vector>
#include <set>
#include <algorithm>
#include <climits>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

class BST {
private:
    Node* root;
    
    // helper function to create new node
    Node* createNode(int data) {
        Node* newNode = new Node;
        newNode->data = data;
        newNode->left = nullptr;
        newNode->right = nullptr;
        return newNode;
    }
    
    // helper for insertion
    Node* insertHelper(Node* node, int data) {
        if (node == nullptr) {
            return createNode(data);
        }
        
        if (data < node->data) {
            node->left = insertHelper(node->left, data);
        } else if (data > node->data) {
            node->right = insertHelper(node->right, data);
        }
        
        return node;
    }
    
    // helper for finding minimum value node
    Node* findMin(Node* node) {
        if (node == nullptr) return nullptr;
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }
    
    // helper for deletion
    Node* deleteHelper(Node* node, int data) {
        if (node == nullptr) {
            return node;
        }
        
        if (data < node->data) {
            node->left = deleteHelper(node->left, data);
        } else if (data > node->data) {
            node->right = deleteHelper(node->right, data);
        } else {
            // node to delete found
            if (node->left == nullptr) {
                Node* temp = node->right;
                delete node;
                return temp;
            } else if (node->right == nullptr) {
                Node* temp = node->left;
                delete node;
                return temp;
            }
            
            // node with two children
            Node* temp = findMin(node->right);
            node->data = temp->data;
            node->right = deleteHelper(node->right, temp->data);
        }
        return node;
    }
    
    // preorder traversal helper
    void preorderHelper(Node* node) {
        if (node != nullptr) {
            cout << node->data << " ";
            preorderHelper(node->left);
            preorderHelper(node->right);
        }
    }
    
    // inorder traversal helper
    void inorderHelper(Node* node) {
        if (node != nullptr) {
            inorderHelper(node->left);
            cout << node->data << " ";
            inorderHelper(node->right);
        }
    }
    
    // postorder traversal helper
    void postorderHelper(Node* node) {
        if (node != nullptr) {
            postorderHelper(node->left);
            postorderHelper(node->right);
            cout << node->data << " ";
        }
    }
    
    // find max helper (rightmost)
    Node* findMaxHelper(Node* node) {
        if (node == nullptr) return nullptr;
        while (node->right != nullptr) {
            node = node->right;
        }
        return node;
    }
    
    // find min helper (leftmost)
    Node* findMinHelper(Node* node) {
        if (node == nullptr) return nullptr;
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }
    
    // check similar structure helper
    bool similarStructureHelper(Node* tree1, Node* tree2) {
        if (tree1 == nullptr && tree2 == nullptr) {
            return true;
        }
        
        if (tree1 == nullptr || tree2 == nullptr) {
            return false;
        }
        
        return similarStructureHelper(tree1->left, tree2->left) && 
               similarStructureHelper(tree1->right, tree2->right);
    }
    
    // find depth of a node helper
    int findDepthHelper(Node* node, int value, int depth) {
        if (node == nullptr) {
            return -1; // not found
        }
        
        if (node->data == value) {
            return depth;
        }
        
        int leftDepth = findDepthHelper(node->left, value, depth + 1);
        if (leftDepth != -1) return leftDepth;
        
        return findDepthHelper(node->right, value, depth + 1);
    }
    
    // calculate height from specific node (CORRECTED)
    int calculateNodeHeight(Node* node) {
        if (node == nullptr) {
            return -1;
        }
        
        int leftHeight = calculateNodeHeight(node->left);
        int rightHeight = calculateNodeHeight(node->right);
        
        return max(leftHeight, rightHeight) + 1;
    }
    
    // find node and return its height (CORRECTED)
    int findNodeHeight(Node* node, int value) {
        if (node == nullptr) {
            return -2; // not found indicator
        }
        
        if (node->data == value) {
            return calculateNodeHeight(node);
        }
        
        int leftResult = findNodeHeight(node->left, value);
        if (leftResult != -2) return leftResult;
        
        return findNodeHeight(node->right, value);
    }
    
    // get tree height
    int getTreeHeight(Node* node) {
        if (node == nullptr) {
            return -1;
        }
        
        int leftHeight = getTreeHeight(node->left);
        int rightHeight = getTreeHeight(node->right);
        
        return max(leftHeight, rightHeight) + 1;
    }
    
    // find node helper
    Node* findNodeHelper(Node* node, int value) {
        if (node == nullptr || node->data == value) {
            return node;
        }
        
        if (value < node->data) {
            return findNodeHelper(node->left, value);
        }
        
        return findNodeHelper(node->right, value);
    }
    
    // find parent helper
    Node* findParentHelper(Node* node, int value) {
        if (node == nullptr || (node->left == nullptr && node->right == nullptr)) {
            return nullptr;
        }
        
        if ((node->left != nullptr && node->left->data == value) ||
            (node->right != nullptr && node->right->data == value)) {
            return node;
        }
        
        if (value < node->data) {
            return findParentHelper(node->left, value);
        } else {
            return findParentHelper(node->right, value);
        }
    }
    
    // check if BST helper
    bool isBSTHelper(Node* node, int minVal, int maxVal) {
        if (node == nullptr) return true;
        
        if (node->data <= minVal || node->data >= maxVal) {
            return false;
        }
        
        return isBSTHelper(node->left, minVal, node->data) &&
               isBSTHelper(node->right, node->data, maxVal);
    }
    
    // count nodes in range helper
    int countInRangeHelper(Node* node, int low, int high) {
        if (node == nullptr) return 0;
        
        int count = 0;
        if (node->data >= low && node->data <= high) {
            count = 1;
        }
        
        count += countInRangeHelper(node->left, low, high);
        count += countInRangeHelper(node->right, low, high);
        
        return count;
    }
    
    // LCA helper
    Node* lcaHelper(Node* node, int n1, int n2) {
        if (node == nullptr) return nullptr;
        
        if (node->data > n1 && node->data > n2) {
            return lcaHelper(node->left, n1, n2);
        }
        
        if (node->data < n1 && node->data < n2) {
            return lcaHelper(node->right, n1, n2);
        }
        
        return node;
    }
    
    // inorder for median
    void inorderForMedian(Node* node, vector<int>& arr) {
        if (node != nullptr) {
            inorderForMedian(node->left, arr);
            arr.push_back(node->data);
            inorderForMedian(node->right, arr);
        }
    }
    
    // collect unique values
    void collectUniqueValues(Node* node, set<int>& uniqueSet) {
        if (node != nullptr) {
            uniqueSet.insert(node->data);
            collectUniqueValues(node->left, uniqueSet);
            collectUniqueValues(node->right, uniqueSet);
        }
    }
    
    // find pair with sum helper
    bool findPairHelper(Node* node, int target, set<int>& visited) {
        if (node == nullptr) return false;
        
        int complement = target - node->data;
        if (visited.find(complement) != visited.end()) {
            cout << "pair found: " << node->data << " + " << complement << " = " << target << endl;
            return true;
        }
        
        visited.insert(node->data);
        
        return findPairHelper(node->left, target, visited) ||
               findPairHelper(node->right, target, visited);
    }
    
    // collect nodes for common elements
    void collectNodes(Node* node, set<int>& nodeSet) {
        if (node != nullptr) {
            nodeSet.insert(node->data);
            collectNodes(node->left, nodeSet);
            collectNodes(node->right, nodeSet);
        }
    }
    
    // find predecessor
    Node* findPredecessor(Node* node, int value) {
        Node* target = findNodeHelper(node, value);
        if (target == nullptr) return nullptr;
        
        // if left subtree exists, predecessor is rightmost in left subtree
        if (target->left != nullptr) {
            return findMaxHelper(target->left);
        }
        
        // otherwise find ancestor where target is in right subtree
        Node* predecessor = nullptr;
        Node* current = root;
        
        while (current != target) {
            if (target->data > current->data) {
                predecessor = current;
                current = current->right;
            } else {
                current = current->left;
            }
        }
        
        return predecessor;
    }
    
    // find successor
    Node* findSuccessor(Node* node, int value) {
        Node* target = findNodeHelper(node, value);
        if (target == nullptr) return nullptr;
        
        // if right subtree exists, successor is leftmost in right subtree
        if (target->right != nullptr) {
            return findMinHelper(target->right);
        }
        
        // otherwise find ancestor where target is in left subtree
        Node* successor = nullptr;
        Node* current = root;
        
        while (current != target) {
            if (target->data < current->data) {
                successor = current;
                current = current->left;
            } else {
                current = current->right;
            }
        }
        
        return successor;
    }

public:
    BST() {
        root = nullptr;
    }
    
    // 1) insertion
    void insert(int data) {
        root = insertHelper(root, data);
        cout << "inserted " << data << " successfully bestie" << endl;
    }
    
    // 2) deletion
    void deleteValue(int data) {
        root = deleteHelper(root, data);
        cout << "deleted " << data << " from tree fr" << endl;
    }
    
    // 3) preorder traversal
    void preorderTraversal() {
        if (root == nullptr) {
            cout << "tree empty as my brain during exams" << endl;
            return;
        }
        cout << "preorder traversal: ";
        preorderHelper(root);
        cout << endl;
    }
    
    // 4) inorder traversal
    void inorderTraversal() {
        if (root == nullptr) {
            cout << "tree empty bestie" << endl;
            return;
        }
        cout << "inorder traversal (sorted): ";
        inorderHelper(root);
        cout << endl;
    }
    
    // 5) postorder traversal
    void postorderTraversal() {
        if (root == nullptr) {
            cout << "tree empty fr fr" << endl;
            return;
        }
        cout << "postorder traversal: ";
        postorderHelper(root);
        cout << endl;
    }
    
    // 6) find max (rightmost)
    void findMax() {
        Node* maxNode = findMaxHelper(root);
        if (maxNode == nullptr) {
            cout << "tree empty, no max value sigma" << endl;
        } else {
            cout << "maximum value (rightmost): " << maxNode->data << endl;
        }
    }
    
    // 7) find min (leftmost)
    void findMin() {
        Node* minNode = findMinHelper(root);
        if (minNode == nullptr) {
            cout << "tree empty, no min value bestie" << endl;
        } else {
            cout << "minimum value (leftmost): " << minNode->data << endl;
        }
    }
    
    // 8) check similar structure with another tree
    bool checkSimilarStructure(BST& otherTree) {
        return similarStructureHelper(root, otherTree.root);
    }
    
    // 9) find depth and height of node (CORRECTED)
    void findDepthAndHeight(int value) {
        int depth = findDepthHelper(root, value, 0);
        if (depth == -1) {
            cout << "value " << value << " not found in tree bestie" << endl;
            return;
        }
        
        int height = findNodeHeight(root, value);
        
        cout << "node " << value << ":" << endl;
        cout << "depth: " << depth << " (distance from root)" << endl;
        cout << "height: " << height << " (distance to deepest leaf)" << endl;
    }
    
    // 10) get tree depth and height
    void getTreeInfo() {
        int height = getTreeHeight(root);
        cout << "tree height: " << height << endl;
        cout << "tree depth: " << height << " (same as height for whole tree)" << endl;
    }
    
    // 11) calculate levels from number of nodes
    void calculateLevels(int numNodes) {
        if (numNodes <= 0) {
            cout << "invalid number of nodes bestie" << endl;
            return;
        }
        
        // minimum levels (balanced tree)
        int minLevels = ceil(log2(numNodes + 1));
        // maximum levels (skewed tree)
        int maxLevels = numNodes;
        
        cout << "with " << numNodes << " nodes:" << endl;
        cout << "minimum levels (balanced): " << minLevels << endl;
        cout << "maximum levels (skewed): " << maxLevels << endl;
    }
    
    // 12) find parent node
    void findParent(int value) {
        if (root == nullptr) {
            cout << "tree empty bestie" << endl;
            return;
        }
        
        if (root->data == value) {
            cout << "node " << value << " is root, no parent fr" << endl;
            return;
        }
        
        Node* parent = findParentHelper(root, value);
        if (parent == nullptr) {
            cout << "node " << value << " not found in tree" << endl;
        } else {
            cout << "parent of " << value << " is: " << parent->data << endl;
        }
    }
    
    // 13) find child nodes
    void findChildren(int value) {
        Node* node = findNodeHelper(root, value);
        if (node == nullptr) {
            cout << "node " << value << " not found bestie" << endl;
            return;
        }
        
        cout << "children of " << value << ": ";
        if (node->left == nullptr && node->right == nullptr) {
            cout << "no children (leaf node)" << endl;
        } else {
            if (node->left != nullptr) {
                cout << "left: " << node->left->data << " ";
            }
            if (node->right != nullptr) {
                cout << "right: " << node->right->data << " ";
            }
            cout << endl;
        }
    }
    
    // 14) left's rightmost node
    void findLeftRightmost(int value) {
        Node* node = findNodeHelper(root, value);
        if (node == nullptr || node->left == nullptr) {
            cout << "node " << value << " has no left subtree bestie" << endl;
            return;
        }
        
        Node* rightmost = findMaxHelper(node->left);
        cout << "left subtree's rightmost node of " << value << ": " << rightmost->data << endl;
    }
    
    // 15) right's leftmost node
    void findRightLeftmost(int value) {
        Node* node = findNodeHelper(root, value);
        if (node == nullptr || node->right == nullptr) {
            cout << "node " << value << " has no right subtree bestie" << endl;
            return;
        }
        
        Node* leftmost = findMinHelper(node->right);
        cout << "right subtree's leftmost node of " << value << ": " << leftmost->data << endl;
    }
    
    // 16) left's leftmost node
    void findLeftLeftmost(int value) {
        Node* node = findNodeHelper(root, value);
        if (node == nullptr || node->left == nullptr) {
            cout << "node " << value << " has no left subtree bestie" << endl;
            return;
        }
        
        Node* leftmost = findMinHelper(node->left);
        cout << "left subtree's leftmost node of " << value << ": " << leftmost->data << endl;
    }
    
    // 17) right's rightmost node
    void findRightRightmost(int value) {
        Node* node = findNodeHelper(root, value);
        if (node == nullptr || node->right == nullptr) {
            cout << "node " << value << " has no right subtree bestie" << endl;
            return;
        }
        
        Node* current = node->right;
        while (current->right != nullptr) {
            current = current->right;
        }
        cout << "right subtree's rightmost node of " << value << ": " << current->data << endl;
    }
    
    // 18) find height and depth when node value is given (same as 9)
    void findNodeHeightDepth(int value) {
        findDepthAndHeight(value);
    }
    
    // 19) check if tree is BST
    void checkIfBST() {
        if (isBSTHelper(root, INT_MIN, INT_MAX)) {
            cout << "tree is a valid BST no cap" << endl;
        } else {
            cout << "tree is NOT a valid BST bestie" << endl;
        }
    }
    
    // 20) count nodes in range
    void countNodesInRange(int low, int high) {
        int count = countInRangeHelper(root, low, high);
        cout << "nodes in range [" << low << ", " << high << "]: " << count << endl;
    }
    
    // 21) search node with depth and height
    void searchNode(int value) {
        Node* found = findNodeHelper(root, value);
        if (found == nullptr) {
            cout << "node " << value << " not found bestie" << endl;
            return;
        }
        
        cout << "node " << value << " found!" << endl;
        findDepthAndHeight(value);
    }
    
    // 22) lowest common ancestor
    void findLCA(int n1, int n2) {
        Node* lca = lcaHelper(root, n1, n2);
        if (lca == nullptr) {
            cout << "LCA not found bestie" << endl;
        } else {
            cout << "lowest common ancestor of " << n1 << " and " << n2 << ": " << lca->data << endl;
        }
    }
    
    // 23) find median
    void findMedian() {
        if (root == nullptr) {
            cout << "tree empty, no median fr" << endl;
            return;
        }
        
        vector<int> sortedValues;
        inorderForMedian(root, sortedValues);
        
        int n = sortedValues.size();
        if (n % 2 == 1) {
            cout << "median: " << sortedValues[n/2] << endl;
        } else {
            double median = (sortedValues[n/2-1] + sortedValues[n/2]) / 2.0;
            cout << "median: " << median << endl;
        }
    }
    
    // 24) unique node values
    void findUniqueValues() {
        if (root == nullptr) {
            cout << "tree empty bestie" << endl;
            return;
        }
        
        set<int> uniqueSet;
        collectUniqueValues(root, uniqueSet);
        
        cout << "number of unique values: " << uniqueSet.size() << endl;
        cout << "unique values: ";
        for (int val : uniqueSet) {
            cout << val << " ";
        }
        cout << endl;
    }
    
    // 25) find pair with sum
    void findPairWithSum(int target) {
        if (root == nullptr) {
            cout << "tree empty bestie" << endl;
            return;
        }
        
        set<int> visited;
        if (!findPairHelper(root, target, visited)) {
            cout << "no pair found with sum " << target << endl;
        }
    }
    
    // 26) common nodes of two BST
    void findCommonNodes(BST& otherTree) {
        if (root == nullptr || otherTree.root == nullptr) {
            cout << "one or both trees empty bestie" << endl;
            return;
        }
        
        set<int> tree1Nodes, tree2Nodes;
        collectNodes(root, tree1Nodes);
        collectNodes(otherTree.root, tree2Nodes);
        
        cout << "common nodes: ";
        bool found = false;
        for (int val : tree1Nodes) {
            if (tree2Nodes.find(val) != tree2Nodes.end()) {
                cout << val << " ";
                found = true;
            }
        }
        if (!found) {
            cout << "no common nodes fr";
        }
        cout << endl;
    }
    
    // 27) predecessor and successor
    void findPredecessorAndSuccessor(int value) {
        Node* pred = findPredecessor(root, value);
        Node* succ = findSuccessor(root, value);
        
        cout << "for node " << value << ":" << endl;
        if (pred == nullptr) {
            cout << "predecessor: none" << endl;
        } else {
            cout << "predecessor: " << pred->data << endl;
        }
        
        if (succ == nullptr) {
            cout << "successor: none" << endl;
        } else {
            cout << "successor: " << succ->data << endl;
        }
    }
    
    // 28) find total number of levels in BST
    void getTotalLevels() {
        if (root == nullptr) {
            cout << "tree empty, 0 levels bestie" << endl;
            return;
        }
        
        int levels = getTreeHeight(root) + 1; // height + 1 = levels
        cout << "total levels in BST: " << levels << endl;
    }
    
    bool isEmpty() {
        return root == nullptr;
    }
    
    Node* getRoot() {
        return root;
    }
    
    // helper to clear tree
    void clearTree(Node* node) {
        if (node != nullptr) {
            clearTree(node->left);
            clearTree(node->right);
            delete node;
        }
    }
    
    ~BST() {
        clearTree(root);
    }
};

int main() {
    BST tree1, tree2;
    int choice, data, value, nodes, low, high, n1, n2, target;
    
    do {
        cout << "\n=== SIGMA BST MEGA MENU ===" << endl;
        cout << "1. Insert data" << endl;
        cout << "2. Delete data by value" << endl;
        cout << "3. Preorder traversal" << endl;
        cout << "4. Inorder traversal" << endl;
        cout << "5. Postorder traversal" << endl;
        cout << "6. Find maximum value" << endl;
        cout << "7. Find minimum value" << endl;
        cout << "8. Check similar structure with tree2" << endl;
        cout << "9. Find depth and height of node" << endl;
        cout << "10. Get tree info (depth/height)" << endl;
        cout << "11. Calculate levels from number of nodes" << endl;
        cout << "12. Find parent node" << endl;
        cout << "13. Find child nodes" << endl;
        cout << "14. Left's rightmost node" << endl;
        cout << "15. Right's leftmost node" << endl;
        cout << "16. Left's leftmost node" << endl;
        cout << "17. Right's rightmost node" << endl;
        cout << "18. Find height/depth of node (alt)" << endl;
        cout << "19. Check if tree is BST" << endl;
        cout << "20. Count nodes in range" << endl;
        cout << "21. Search node with info" << endl;
        cout << "22. Find LCA of two nodes" << endl;
        cout << "23. Find median of BST" << endl;
        cout << "24. Find unique values" << endl;
        cout << "25. Find pair with sum" << endl;
        cout << "26. Find common nodes with tree2" << endl;
        cout << "27. Find predecessor and successor" << endl;
        cout << "28. Find total levels in BST" << endl;
        cout << "29. Operations on tree2" << endl;
        cout << "0. Exit (touch grass)" << endl;
        cout << "Enter your choice sigma: ";
        cin >> choice;
        
        switch (choice) {
            case 1:
                cout << "Enter data to insert: ";
                cin >> data;
                tree1.insert(data);
                break;
                
            case 2:
                cout << "Enter value to delete: ";
                cin >> data;
                tree1.deleteValue(data);
                break;
                
            case 3:
                tree1.preorderTraversal();
                break;
                
            case 4:
                tree1.inorderTraversal();
                break;
                
            case 5:
                tree1.postorderTraversal();
                break;
                
            case 6:
                tree1.findMax();
                break;
                
            case 7:
                tree1.findMin();
                break;
                
            case 8:
                if (tree1.checkSimilarStructure(tree2)) {
                    cout << "trees have similar structure no cap" << endl;
                } else {
                    cout << "trees have different structure bestie" << endl;
                }
                break;
                
            case 9:
                cout << "Enter node value to find depth/height: ";
                cin >> value;
                tree1.findDepthAndHeight(value);
                break;
                
            case 10:
                tree1.getTreeInfo();
                break;
                
            case 11:
                cout << "Enter number of nodes: ";
                cin >> nodes;
                tree1.calculateLevels(nodes);
                break;
                
            case 12:
                cout << "Enter node value to find parent: ";
                cin >> value;
                tree1.findParent(value);
                break;
                
            case 13:
                cout << "Enter node value to find children: ";
                cin >> value;
                tree1.findChildren(value);
                break;
                
            case 14:
                cout << "Enter node value: ";
                cin >> value;
                tree1.findLeftRightmost(value);
                break;
                
            case 15:
                cout << "Enter node value: ";
                cin >> value;
                tree1.findRightLeftmost(value);
                break;
                
            case 16:
                cout << "Enter node value: ";
                cin >> value;
                tree1.findLeftLeftmost(value);
                break;
                
            case 17:
                cout << "Enter node value: ";
                cin >> value;
                tree1.findRightRightmost(value);
                break;
                
            case 18:
                cout << "Enter node value: ";
                cin >> value;
                tree1.findNodeHeightDepth(value);
                break;
                
            case 19:
                tree1.checkIfBST();
                break;
                
            case 20:
                cout << "Enter range [low high]: ";
                cin >> low >> high;
                tree1.countNodesInRange(low, high);
                break;
                
            case 21:
                cout << "Enter value to search: ";
                cin >> value;
                tree1.searchNode(value);
                break;
                
            case 22:
                cout << "Enter two node values: ";
                cin >> n1 >> n2;
                tree1.findLCA(n1, n2);
                break;
                
            case 23:
                tree1.findMedian();
                break;
                
            case 24:
                tree1.findUniqueValues();
                break;
                
            case 25:
                cout << "Enter target sum: ";
                cin >> target;
                tree1.findPairWithSum(target);
                break;
                
            case 26:
                tree1.findCommonNodes(tree2);
                break;
                
            case 27:
                cout << "Enter node value: ";
                cin >> value;
                tree1.findPredecessorAndSuccessor(value);
                break;
                
            case 28:
                tree1.getTotalLevels();
                break;
                
            case 29:
                cout << "\n=== TREE2 OPERATIONS ===" << endl;
                cout << "1. Insert to tree2" << endl;
                cout << "2. Display tree2 inorder" << endl;
                cout << "Enter sub-choice: ";
                int subChoice;
                cin >> subChoice;
                
                if (subChoice == 1) {
                    cout << "Enter data for tree2: ";
                    cin >> data;
                    tree2.insert(data);
                } else if (subChoice == 2) {
                    cout << "Tree2 ";
                    tree2.inorderTraversal();
                } else {
                    cout << "invalid sub-choice bestie" << endl;
                }
                break;
                
            case 0:
                cout << "exiting... time to touch grass fr" << endl;
                break;
                
            default:
                cout << "invalid choice bestie, try again" << endl;
        }
    } while (choice != 0);
    
    return 0;
}

```
