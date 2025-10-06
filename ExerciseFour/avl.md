#include <iostream>
#include <queue>
#include <vector>
#include <cmath>
#include <iomanip>
#include <algorithm>
using namespace std;

class Node {
public:
    int data;
    Node* left;
    Node* right;
    int height;
    
    Node(int val) {
        data = val;
        left = nullptr;
        right = nullptr;
        height = 1;
    }
};

class AVLTree {
public:
    Node* root;
    
    AVLTree() {
        root = nullptr;
    }
    
    int getHeight(Node* node) {
        if (node == nullptr) return 0;
        return node->height;
    }
    
    int getBalanceFactor(Node* node) {
        if (node == nullptr) return 0;
        return getHeight(node->left) - getHeight(node->right);
    }
    
    Node* rightRotate(Node* y) {
        Node* x = y->left;
        Node* T2 = x->right;
        
        x->right = y;
        y->left = T2;
        
        y->height = max(getHeight(y->left), getHeight(y->right)) + 1;
        x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
        
        return x;
    }
    
    Node* leftRotate(Node* x) {
        Node* y = x->right;
        Node* T2 = y->left;
        
        y->left = x;
        x->right = T2;
        
        x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
        y->height = max(getHeight(y->left), getHeight(y->right)) + 1;
        
        return y;
    }
    
    Node* insert(Node* node, int val) {
        if (node == nullptr) {
            return new Node(val);
        }
        
        if (val < node->data) {
            node->left = insert(node->left, val);
        } else if (val > node->data) {
            node->right = insert(node->right, val);
        } else {
            return node;
        }
        
        node->height = 1 + max(getHeight(node->left), getHeight(node->right));
        
        int balance = getBalanceFactor(node);
        
        if (balance > 1 && val < node->left->data) {
            return rightRotate(node);
        }
        
        if (balance < -1 && val > node->right->data) {
            return leftRotate(node);
        }
        
        if (balance > 1 && val > node->left->data) {
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }
        
        if (balance < -1 && val < node->right->data) {
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }
        
        return node;
    }
    
    Node* minValueNode(Node* node) {
        Node* current = node;
        while (current->left != nullptr) {
            current = current->left;
        }
        return current;
    }
    
    Node* maxValueNode(Node* node) {
        Node* current = node;
        while (current->right != nullptr) {
            current = current->right;
        }
        return current;
    }
    
    Node* deleteNode(Node* root, int val) {
        if (root == nullptr) return root;
        
        if (val < root->data) {
            root->left = deleteNode(root->left, val);
        } else if (val > root->data) {
            root->right = deleteNode(root->right, val);
        } else {
            if (root->left == nullptr || root->right == nullptr) {
                Node* temp = root->left ? root->left : root->right;
                
                if (temp == nullptr) {
                    temp = root;
                    root = nullptr;
                } else {
                    *root = *temp;
                }
                delete temp;
            } else {
                Node* temp = minValueNode(root->right);
                root->data = temp->data;
                root->right = deleteNode(root->right, temp->data);
            }
        }
        
        if (root == nullptr) return root;
        
        root->height = 1 + max(getHeight(root->left), getHeight(root->right));
        
        int balance = getBalanceFactor(root);
        
        if (balance > 1 && getBalanceFactor(root->left) >= 0) {
            return rightRotate(root);
        }
        
        if (balance > 1 && getBalanceFactor(root->left) < 0) {
            root->left = leftRotate(root->left);
            return rightRotate(root);
        }
        
        if (balance < -1 && getBalanceFactor(root->right) <= 0) {
            return leftRotate(root);
        }
        
        if (balance < -1 && getBalanceFactor(root->right) > 0) {
            root->right = rightRotate(root->right);
            return leftRotate(root);
        }
        
        return root;
    }
    
    Node* findNode(Node* root, int val) {
        if (root == nullptr || root->data == val) return root;
        if (val < root->data) return findNode(root->left, val);
        return findNode(root->right, val);
    }
    
    Node* findInorderSuccessor(Node* root, int val) {
        Node* target = findNode(root, val);
        if (target == nullptr) return nullptr;
        
        if (target->right != nullptr) {
            return minValueNode(target->right);
        }
        
        Node* successor = nullptr;
        Node* current = root;
        
        while (current != nullptr) {
            if (val < current->data) {
                successor = current;
                current = current->left;
            } else if (val > current->data) {
                current = current->right;
            } else {
                break;
            }
        }
        
        return successor;
    }
    
    Node* findInorderPredecessor(Node* root, int val) {
        Node* target = findNode(root, val);
        if (target == nullptr) return nullptr;
        
        if (target->left != nullptr) {
            return maxValueNode(target->left);
        }
        
        Node* predecessor = nullptr;
        Node* current = root;
        
        while (current != nullptr) {
            if (val > current->data) {
                predecessor = current;
                current = current->right;
            } else if (val < current->data) {
                current = current->left;
            } else {
                break;
            }
        }
        
        return predecessor;
    }
    
    void printNodesInRange(Node* root, int start, int end) {
        if (root == nullptr) return;
        
        if (start < root->data) {
            printNodesInRange(root->left, start, end);
        }
        
        if (start <= root->data && root->data <= end) {
            cout << root->data << " ";
        }
        
        if (end > root->data) {
            printNodesInRange(root->right, start, end);
        }
    }
    
    int countNodesInRange(Node* root, int start, int end) {
        if (root == nullptr) return 0;
        
        int count = 0;
        
        if (start < root->data) {
            count += countNodesInRange(root->left, start, end);
        }
        
        if (start <= root->data && root->data <= end) {
            count++;
        }
        
        if (end > root->data) {
            count += countNodesInRange(root->right, start, end);
        }
        
        return count;
    }
    
    int findLevel(Node* root, int val, int level) {
        if (root == nullptr) return -1;
        
        if (root->data == val) return level;
        
        int leftLevel = findLevel(root->left, val, level + 1);
        if (leftLevel != -1) return leftLevel;
        
        return findLevel(root->right, val, level + 1);
    }
    
    bool findPath(Node* root, int val, vector<int>& path) {
        if (root == nullptr) return false;
        
        path.push_back(root->data);
        
        if (root->data == val) return true;
        
        if (findPath(root->left, val, path) || findPath(root->right, val, path)) {
            return true;
        }
        
        path.pop_back();
        return false;
    }
    
    bool isCompleteTree(Node* root) {
        if (root == nullptr) return true;
        
        queue<Node*> q;
        q.push(root);
        bool nullFound = false;
        
        while (!q.empty()) {
            Node* current = q.front();
            q.pop();
            
            if (current == nullptr) {
                nullFound = true;
            } else {
                if (nullFound) return false;
                
                q.push(current->left);
                q.push(current->right);
            }
        }
        
        return true;
    }
    
    void findNodesWithOneChild(Node* root, vector<int>& nodes) {
        if (root == nullptr) return;
        
        if ((root->left != nullptr && root->right == nullptr) || 
            (root->left == nullptr && root->right != nullptr)) {
            nodes.push_back(root->data);
        }
        
        findNodesWithOneChild(root->left, nodes);
        findNodesWithOneChild(root->right, nodes);
    }
    
    void findLeafNodes(Node* root, vector<int>& leaves) {
        if (root == nullptr) return;
        
        if (root->left == nullptr && root->right == nullptr) {
            leaves.push_back(root->data);
        }
        
        findLeafNodes(root->left, leaves);
        findLeafNodes(root->right, leaves);
    }
    
    bool isValidAVL(Node* root) {
        if (root == nullptr) return true;
        
        int balance = getBalanceFactor(root);
        
        if (balance > 1 || balance < -1) return false;
        
        return isValidAVL(root->left) && isValidAVL(root->right);
    }
    
    Node* sortedArrayToAVL(vector<int>& arr, int start, int end) {
        if (start > end) return nullptr;
        
        int mid = start + (end - start) / 2;
        Node* node = new Node(arr[mid]);
        
        node->left = sortedArrayToAVL(arr, start, mid - 1);
        node->right = sortedArrayToAVL(arr, mid + 1, end);
        
        node->height = 1 + max(getHeight(node->left), getHeight(node->right));
        
        return node;
    }
    
    void storeInorder(Node* root, vector<int>& nodes) {
        if (root == nullptr) return;
        storeInorder(root->left, nodes);
        nodes.push_back(root->data);
        storeInorder(root->right, nodes);
    }
    
    Node* mergeTrees(Node* root1, Node* root2) {
        vector<int> nodes1, nodes2;
        storeInorder(root1, nodes1);
        storeInorder(root2, nodes2);
        
        vector<int> merged;
        int i = 0, j = 0;
        
        while (i < nodes1.size() && j < nodes2.size()) {
            if (nodes1[i] < nodes2[j]) {
                merged.push_back(nodes1[i++]);
            } else if (nodes1[i] > nodes2[j]) {
                merged.push_back(nodes2[j++]);
            } else {
                merged.push_back(nodes1[i++]);
                j++;
            }
        }
        
        while (i < nodes1.size()) {
            merged.push_back(nodes1[i++]);
        }
        
        while (j < nodes2.size()) {
            merged.push_back(nodes2[j++]);
        }
        
        return sortedArrayToAVL(merged, 0, merged.size() - 1);
    }
    
    Node* findLCA(Node* root, int n1, int n2) {
        if (root == nullptr) return nullptr;
        
        if (root->data > n1 && root->data > n2) {
            return findLCA(root->left, n1, n2);
        }
        
        if (root->data < n1 && root->data < n2) {
            return findLCA(root->right, n1, n2);
        }
        
        return root;
    }
    
    void displayLevelOrder(Node* root) {
        if (root == nullptr) {
            cout << "Tree is empty fr 💀" << endl;
            return;
        }
        
        queue<Node*> q;
        q.push(root);
        
        while (!q.empty()) {
            int levelSize = q.size();
            
            for (int i = 0; i < levelSize; i++) {
                Node* current = q.front();
                q.pop();
                
                cout << current->data << " ";
                
                if (current->left != nullptr) q.push(current->left);
                if (current->right != nullptr) q.push(current->right);
            }
            cout << endl;
        }
    }
    
    void displayArrayStructure(Node* root) {
        if (root == nullptr) {
            cout << "Tree is empty no cap 💀" << endl;
            return;
        }
        
        vector<int> arr;
        queue<Node*> q;
        q.push(root);
        
        while (!q.empty()) {
            Node* current = q.front();
            q.pop();
            
            if (current != nullptr) {
                arr.push_back(current->data);
                q.push(current->left);
                q.push(current->right);
            } else {
                arr.push_back(-1);
            }
        }
        
        while (!arr.empty() && arr.back() == -1) {
            arr.pop_back();
        }
        
        cout << "Array representation (null = -1): ";
        for (int i = 0; i < arr.size(); i++) {
            cout << arr[i] << " ";
        }
        cout << endl;
    }
    void displayPyramid(Node* root) {
        if (root == nullptr) {
            cout << "Tree is empty fr 💀" << endl;
            return;
        }
        
        int h = getHeight(root);
        int maxWidth = pow(2, h) - 1;
        
        queue<Node*> q;
        q.push(root);
        
        for (int level = 0; level < h; level++) {
            int levelNodes = pow(2, level);
            int spaceBetween = maxWidth / pow(2, level);
            int leadingSpace = (maxWidth - (levelNodes - 1) * spaceBetween) / 2;
            
            vector<Node*> currentLevel;
            for (int i = 0; i < levelNodes; i++) {
                if (!q.empty()) {
                    currentLevel.push_back(q.front());
                    q.pop();
                } else {
                    currentLevel.push_back(nullptr);
                }
            }
            
            for (int i = 0; i < leadingSpace; i++) {
                cout << " ";
            }
            
            for (int i = 0; i < currentLevel.size(); i++) {
                if (currentLevel[i] != nullptr) {
                    cout << setw(2) << currentLevel[i]->data;
                    q.push(currentLevel[i]->left);
                    q.push(currentLevel[i]->right);
                } else {
                    cout << "  ";
                    q.push(nullptr);
                    q.push(nullptr);
                }
                
                if (i < currentLevel.size() - 1) {
                    for (int j = 0; j < spaceBetween; j++) {
                        cout << " ";
                    }
                }
            }
            cout << endl;
            
            if (level < h - 1) {
                for (int i = 0; i < leadingSpace - 1; i++) {
                    cout << " ";
                }
                
                for (int i = 0; i < currentLevel.size(); i++) {
                    if (currentLevel[i] != nullptr) {
                        if (currentLevel[i]->left != nullptr) {
                            cout << "/";
                        } else {
                            cout << " ";
                        }
                        cout << " ";
                        if (currentLevel[i]->right != nullptr) {
                            cout << "\\";
                        } else {
                            cout << " ";
                        }
                    } else {
                        cout << "   ";
                    }
                    
                    if (i < currentLevel.size() - 1) {
                        for (int j = 0; j < spaceBetween - 2; j++) {
                            cout << " ";
                        }
                    }
                }
                cout << endl;
            }
        }
    }
    
    
    int getBalanceFactorOfNode(Node* root, int val) {
        if (root == nullptr) return -9999;
        
        if (root->data == val) {
            return getBalanceFactor(root);
        }
        
        if (val < root->data) {
            return getBalanceFactorOfNode(root->left, val);
        } else {
            return getBalanceFactorOfNode(root->right, val);
        }
    }
    
    int getTreeHeight() {
        return getHeight(root);
    }
    
    void preorder(Node* node) {
        if (node == nullptr) return;
        cout << node->data << " ";
        preorder(node->left);
        preorder(node->right);
    }
    
    void inorder(Node* node) {
        if (node == nullptr) return;
        inorder(node->left);
        cout << node->data << " ";
        inorder(node->right);
    }
    
    void postorder(Node* node) {
        if (node == nullptr) return;
        postorder(node->left);
        postorder(node->right);
        cout << node->data << " ";
    }
    
    bool areStructurallySimilar(Node* root1, Node* root2) {
        if (root1 == nullptr && root2 == nullptr) return true;
        if (root1 == nullptr || root2 == nullptr) return false;
        
        return areStructurallySimilar(root1->left, root2->left) && 
               areStructurallySimilar(root1->right, root2->right);
    }
    
    bool search(Node* root, int val) {
        if (root == nullptr) return false;
        if (root->data == val) return true;
        
        if (val < root->data) {
            return search(root->left, val);
        } else {
            return search(root->right, val);
        }
    }
    
    int findMin() {
        if (root == nullptr) {
            cout << "Tree is empty bruh" << endl;
            return -1;
        }
        return minValueNode(root)->data;
    }
    
    int findMax() {
        if (root == nullptr) {
            cout << "Tree is empty gang" << endl;
            return -1;
        }
        return maxValueNode(root)->data;
    }
};

int main() {
    AVLTree tree;
    AVLTree tree2;
    int choice, value, value2;
    
    while (true) {
        cout << "\n🔥 AVL Tree Menu (Ultimate Edition) 🔥" << endl;
        cout << "1. Insert value" << endl;
        cout << "2. Delete value" << endl;
        cout << "3. Display tree level by level" << endl;
        cout << "4. Display array structure" << endl;
        cout << "5. Get balance factor of node" << endl;
        cout << "6. Display height of tree" << endl;
        cout << "7. Preorder traversal" << endl;
        cout << "8. Inorder traversal" << endl;
        cout << "9. Postorder traversal" << endl;
        cout << "10. Check if two trees structurally similar" << endl;
        cout << "11. Search for a node" << endl;
        cout << "12. Find minimum value" << endl;
        cout << "13. Find maximum value" << endl;
        cout << "14. Display pyramid structure" << endl;
        cout << "15. Find inorder successor" << endl;
        cout << "16. Find inorder predecessor" << endl;
        cout << "17. Print nodes in range" << endl;
        cout << "18. Count nodes in range" << endl;
        cout << "19. Find level of node" << endl;
        cout << "20. Find path from root to node" << endl;
        cout << "21. Check if tree is complete binary tree" << endl;
        cout << "22. Display nodes with one child" << endl;
        cout << "23. Display leaf nodes" << endl;
        cout << "24. Check if valid AVL tree" << endl;
        cout << "25. Convert sorted array to AVL" << endl;
        cout << "26. Merge two AVL trees" << endl;
        cout << "27. Find lowest common ancestor" << endl;
        cout << "0. Exit" << endl;
        cout << "Enter choice: ";
        cin >> choice;
        
        switch(choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value;
                tree.root = tree.insert(tree.root, value);
                cout << "Value inserted successfully 🚀" << endl;
                break;
                
            case 2:
                cout << "Enter value to delete: ";
                cin >> value;
                if (tree.search(tree.root, value)) {
                    tree.root = tree.deleteNode(tree.root, value);
                    cout << "Value deleted successfully 💯" << endl;
                } else {
                    cout << "Value not found in tree ngl 😬" << endl;
                }
                break;
                
            case 3:
                cout << "Tree structure (level by level):" << endl;
                tree.displayLevelOrder(tree.root);
                break;
                
            case 4:
                tree.displayArrayStructure(tree.root);
                break;
                
            case 5:
                cout << "Enter node value to check balance factor: ";
                cin >> value;
                if (tree.search(tree.root, value)) {
                    int bf = tree.getBalanceFactorOfNode(tree.root, value);
                    cout << "Balance factor of node " << value << " is: " << bf;
                    if (bf > 1 || bf < -1) {
                        cout << " (this node kinda sus, not balanced fr 🤨)";
                    } else {
                        cout << " (perfectly balanced ✨)";
                    }
                    cout << endl;
                } else {
                    cout << "Node not found in tree my guy 🫠" << endl;
                }
                break;
                
            case 6:
                cout << "Height of tree: " << tree.getTreeHeight() << endl;
                break;
                
            case 7:
                cout << "Preorder traversal: ";
                if (tree.root == nullptr) {
                    cout << "Tree empty bestie" << endl;
                } else {
                    tree.preorder(tree.root);
                    cout << endl;
                }
                break;
                
            case 8:
                cout << "Inorder traversal: ";
                if (tree.root == nullptr) {
                    cout << "Tree empty bruh" << endl;
                } else {
                    tree.inorder(tree.root);
                    cout << endl;
                }
                break;
                
            case 9:
                cout << "Postorder traversal: ";
                if (tree.root == nullptr) {
                    cout << "Tree empty gang" << endl;
                } else {
                    tree.postorder(tree.root);
                    cout << endl;
                }
                break;
                
            case 10:
                {
                    cout << "Enter number of nodes for second tree: ";
                    int n;
                    cin >> n;
                    cout << "Enter values for second tree:" << endl;
                    for (int i = 0; i < n; i++) {
                        cin >> value;
                        tree2.root = tree2.insert(tree2.root, value);
                    }
                    if (tree.areStructurallySimilar(tree.root, tree2.root)) {
                        cout << "Trees are structurally similar fr fr ✅" << endl;
                    } else {
                        cout << "Trees are NOT structurally similar, different vibes 🚫" << endl;
                    }
                }
                break;
                
            case 11:
                cout << "Enter value to search: ";
                cin >> value;
                if (tree.search(tree.root, value)) {
                    cout << "Node found in tree 🎯" << endl;
                } else {
                    cout << "Node not found in tree 😔" << endl;
                }
                break;
                
            case 12:
                {
                    int minVal = tree.findMin();
                    if (minVal != -1) {
                        cout << "Minimum value in tree: " << minVal << " 📉" << endl;
                    }
                }
                break;
                
            case 13:
                {
                    int maxVal = tree.findMax();
                    if (maxVal != -1) {
                        cout << "Maximum value in tree: " << maxVal << " 📈" << endl;
                    }
                }
                break;
                
            case 14:
                cout << "Pyramid structure of tree:" << endl;
                tree.displayPyramid(tree.root);
                break;
                
            case 15:
                cout << "Enter node value to find successor: ";
                cin >> value;
                if (tree.search(tree.root, value)) {
                    Node* successor = tree.findInorderSuccessor(tree.root, value);
                    if (successor != nullptr) {
                        cout << "Inorder successor of " << value << " is: " << successor->data << " 📍" << endl;
                    } else {
                        cout << "No successor exists (already the max value fr) 🔚" << endl;
                    }
                } else {
                    cout << "Node not found in tree bestie 🫠" << endl;
                }
                break;
                
            case 16:
                cout << "Enter node value to find predecessor: ";
                cin >> value;
                if (tree.search(tree.root, value)) {
                    Node* predecessor = tree.findInorderPredecessor(tree.root, value);
                    if (predecessor != nullptr) {
                        cout << "Inorder predecessor of " << value << " is: " << predecessor->data << " 📍" << endl;
                    } else {
                        cout << "No predecessor exists (already the min value fr) 🔚" << endl;
                    }
                } else {
                    cout << "Node not found in tree bestie 🫠" << endl;
                }
                break;
                
            case 17:
                cout << "Enter start of range: ";
                cin >> value;
                cout << "Enter end of range: ";
                cin >> value2;
                cout << "Nodes in range [" << value << ", " << value2 << "]: ";
                tree.printNodesInRange(tree.root, value, value2);
                cout << endl;
                break;
                
            case 18:
                cout << "Enter start of range: ";
                cin >> value;
                cout << "Enter end of range: ";
                cin >> value2;
                {
                    int count = tree.countNodesInRange(tree.root, value, value2);
                    cout << "Number of nodes in range [" << value << ", " << value2 << "]: " << count << " 🔢" << endl;
                }
                break;
                
            case 19:
                cout << "Enter node value to find level: ";
                cin >> value;
                {
                    int level = tree.findLevel(tree.root, value, 0);
                    if (level != -1) {
                        cout << "Level of node " << value << " is: " << level << " (root is level 0) 📊" << endl;
                    } else {
                        cout << "Node not found in tree gang 😔" << endl;
                    }
                }
                break;
                
            case 20:
                cout << "Enter node value to find path: ";
                cin >> value;
                {
                    vector<int> path;
                    if (tree.findPath(tree.root, value, path)) {
                        cout << "Path from root to " << value << ": ";
                        for (int i = 0; i < path.size(); i++) {
                            cout << path[i];
                            if (i < path.size() - 1) cout << " -> ";
                        }
                        cout << " 🛤️" << endl;
                    } else {
                        cout << "Node not found in tree fam 🫠" << endl;
                    }
                }
                break;
                
            case 21:
                if (tree.isCompleteTree(tree.root)) {
                    cout << "Tree is a complete binary tree ✅" << endl;
                } else {
                    cout << "Tree is NOT a complete binary tree ❌" << endl;
                }
                break;
                
            case 22:
                {
                    vector<int> nodes;
                    tree.findNodesWithOneChild(tree.root, nodes);
                    cout << "Nodes with exactly one child: ";
                    if (nodes.empty()) {
                        cout << "None found 🚫" << endl;
                    } else {
                        cout << "[ ";
                        for (int i = 0; i < nodes.size(); i++) {
                            cout << nodes[i];
                            if (i < nodes.size() - 1) cout << ", ";
                        }
                        cout << " ] (" << nodes.size() << " nodes) 📦" << endl;
                    }
                }
                break;
                
            case 23:
                {
                    vector<int> leaves;
                    tree.findLeafNodes(tree.root, leaves);
                    cout << "Leaf nodes (no children): ";
                    if (leaves.empty()) {
                        cout << "None found 🚫" << endl;
                    } else {
                        cout << "[ ";
                        for (int i = 0; i < leaves.size(); i++) {
                            cout << leaves[i];
                            if (i < leaves.size() - 1) cout << ", ";
                        }
                        cout << " ] (" << leaves.size() << " leaves) 🍃" << endl;
                    }
                }
                break;
                
            case 24:
                if (tree.isValidAVL(tree.root)) {
                    cout << "Tree is a valid AVL tree (all balance factors within range) ✅" << endl;
                } else {
                    cout << "Tree is NOT a valid AVL tree (some nodes unbalanced) ❌" << endl;
                }
                break;
                
            case 25:
                {
                    cout << "Enter number of elements in sorted array: ";
                    int n;
                    cin >> n;
                    vector<int> arr(n);
                    cout << "Enter sorted array elements:" << endl;
                    for (int i = 0; i < n; i++) {
                        cin >> arr[i];
                    }
                    tree.root = tree.sortedArrayToAVL(arr, 0, n - 1);
                    cout << "AVL tree created from sorted array 🌳" << endl;
                }
                break;
                
            case 26:
                {
                    cout << "Enter number of nodes for second tree: ";
                    int n;
                    cin >> n;
                    cout << "Enter values for second tree:" << endl;
                    for (int i = 0; i < n; i++) {
                        cin >> value;
                        tree2.root = tree2.insert(tree2.root, value);
                    }
                    tree.root = tree.mergeTrees(tree.root, tree2.root);
                    cout << "Trees merged successfully (duplicates removed) 🔗" << endl;
                }
                break;
                
            case 27:
                cout << "Enter first node value: ";
                cin >> value;
                cout << "Enter second node value: ";
                cin >> value2;
                if (tree.search(tree.root, value) && tree.search(tree.root, value2)) {
                    Node* lca = tree.findLCA(tree.root, value, value2);
                    if (lca != nullptr) {
                        cout << "Lowest common ancestor of " << value << " and " << value2 << " is: " << lca->data << " 🎯" << endl;
                    } else {
                        cout << "Could not find LCA somehow 🤔" << endl;
                    }
                } else {
                    cout << "One or both nodes not found in tree bruh 😬" << endl;
                }
                break;
                
            case 0:
                cout << "Exiting program. Catch you later fam 👋" << endl;
                return 0;
                
            default:
                cout << "Invalid choice npc behavior detected 💀 Try again" << endl;
        }
    }
    
    return 0;
}
