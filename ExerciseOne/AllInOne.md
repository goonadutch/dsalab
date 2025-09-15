# CODE :

```

#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
    
    Node(int val) : data(val), next(nullptr) {}
};

class LinkedList {
private:
    Node* head;

public:
    LinkedList() : head(nullptr) {}
    
    // a) insertion of data at end
    void insertAtEnd(int data) {
        Node* newNode = new Node(data);
        if (head == nullptr) {
            head = newNode;
            return;
        }
        
        Node* temp = head;
        while (temp->next != nullptr) {
            temp = temp->next;
        }
        temp->next = newNode;
    }
    
    // d) insertion of data at the front
    void insertAtFront(int data) {
        Node* newNode = new Node(data);
        newNode->next = head;
        head = newNode;
    }
    
    // b) deletion of data by value
    void deleteByValue(int data) {
        if (head == nullptr) {
            cout << "bruh list is empty fr" << endl;
            return;
        }
        
        if (head->data == data) {
            Node* temp = head;
            head = head->next;
            delete temp;
            cout << "deleted " << data << " successfully" << endl;
            return;
        }
        
        Node* current = head;
        while (current->next != nullptr && current->next->data != data) {
            current = current->next;
        }
        
        if (current->next == nullptr) {
            cout << "nah fam " << data << " ain't in the list" << endl;
            return;
        }
        
        Node* temp = current->next;
        current->next = current->next->next;
        delete temp;
        cout << "yeeted " << data << " from the list" << endl;
    }
    
    // deletion by position
    void deleteByPosition(int pos) {
        if (head == nullptr) {
            cout << "list empty bestie" << endl;
            return;
        }
        
        if (pos == 1) {
            Node* temp = head;
            head = head->next;
            delete temp;
            cout << "deleted node at position 1" << endl;
            return;
        }
        
        Node* current = head;
        for (int i = 1; i < pos - 1 && current->next != nullptr; i++) {
            current = current->next;
        }
        
        if (current->next == nullptr) {
            cout << "no node at position " << pos << " sigma" << endl;
            return;
        }
        
        Node* temp = current->next;
        current->next = current->next->next;
        delete temp;
        cout << "deleted node at position " << pos << endl;
    }
    
    // c) display of linked list
    void display() {
        if (head == nullptr) {
            cout << "list is emptier than my rizz game" << endl;
            return;
        }
        
        Node* temp = head;
        cout << "linked list be like: ";
        while (temp != nullptr) {
            cout << temp->data;
            if (temp->next != nullptr) cout << " -> ";
            temp = temp->next;
        }
        cout << " -> null" << endl;
    }
    
    // e) count number of elements
    int countElements() {
        int count = 0;
        Node* temp = head;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }
    
    // f) print the data when number of the node is given
    void printNthNode(int n) {
        if (head == nullptr) {
            cout << "no " << n << "th node" << endl;
            return;
        }
        
        Node* temp = head;
        for (int i = 1; i < n && temp != nullptr; i++) {
            temp = temp->next;
        }
        
        if (temp == nullptr) {
            cout << "no " << n << "th node" << endl;
        } else {
            cout << n << "th node data: " << temp->data << endl;
        }
    }
    
    // g) insert data at the node number
    void insertAtPosition(int data, int pos) {
        if (pos == 1) {
            insertAtFront(data);
            return;
        }
        
        Node* newNode = new Node(data);
        Node* current = head;
        
        for (int i = 1; i < pos - 1 && current != nullptr; i++) {
            current = current->next;
        }
        
        if (current == nullptr) {
            // extend the list
            insertAtEnd(data);
            cout << "kept in " << countElements() << "th node" << endl;
        } else {
            newNode->next = current->next;
            current->next = newNode;
            cout << "inserted " << data << " at position " << pos << endl;
        }
    }
    
    // h) print the data of the last node
    void printLastNode() {
        if (head == nullptr) {
            cout << "no last node cuz list empty bestie" << endl;
            return;
        }
        
        Node* temp = head;
        while (temp->next != nullptr) {
            temp = temp->next;
        }
        cout << "last node data: " << temp->data << endl;
    }
    
    // i) keep the linked list sorted everytime - sorted insertion
    void insertSorted(int data) {
        Node* newNode = new Node(data);
        
        if (head == nullptr || head->data > data) {
            newNode->next = head;
            head = newNode;
            return;
        }
        
        Node* current = head;
        while (current->next != nullptr && current->next->data < data) {
            current = current->next;
        }
        
        newNode->next = current->next;
        current->next = newNode;
    }
    
    // j) reverse a linked list
    void reverse() {
        if (head == nullptr) {
            cout << "can't reverse empty list fr" << endl;
            return;
        }
        
        Node* prev = nullptr;
        Node* current = head;
        Node* next = nullptr;
        
        while (current != nullptr) {
            next = current->next;
            current->next = prev;
            prev = current;
            current = next;
        }
        
        head = prev;
        cout << "list reversed successfully no cap" << endl;
    }
    
    // k) sort a linked list in ascending order (bubble sort approach)
    void sortList() {
        if (head == nullptr || head->next == nullptr) {
            cout << "list already sorted bestie" << endl;
            return;
        }
        
        bool swapped;
        do {
            swapped = false;
            Node* current = head;
            
            while (current->next != nullptr) {
                if (current->data > current->next->data) {
                    // swap data
                    int temp = current->data;
                    current->data = current->next->data;
                    current->next->data = temp;
                    swapped = true;
                }
                current = current->next;
            }
        } while (swapped);
        
        cout << "list sorted in ascending order fr fr" << endl;
    }
    
    ~LinkedList() {
        while (head != nullptr) {
            Node* temp = head;
            head = head->next;
            delete temp;
        }
    }
};

int main() {
    LinkedList list;
    int choice, data, pos;
    
    do {
        cout << "\n=== SIGMA LINKED LIST MENU ===" << endl;
        cout << "1. Insert data at end" << endl;
        cout << "2. Delete data by value" << endl;
        cout << "3. Display linked list" << endl;
        cout << "4. Insert data at front" << endl;
        cout << "5. Count number of elements" << endl;
        cout << "6. Print data of nth node" << endl;
        cout << "7. Insert data at specific position" << endl;
        cout << "8. Print data of last node" << endl;
        cout << "9. Insert data keeping list sorted" << endl;
        cout << "10. Reverse linked list" << endl;
        cout << "11. Sort linked list" << endl;
        cout << "12. Delete by position" << endl;
        cout << "0. Exit (touch grass)" << endl;
        cout << "Enter your choice sigma: ";
        cin >> choice;
        
        switch (choice) {
            case 1:
                cout << "Enter data to insert: ";
                cin >> data;
                list.insertAtEnd(data);
                cout << "data inserted at end successfully" << endl;
                break;
                
            case 2:
                cout << "Enter data to delete: ";
                cin >> data;
                list.deleteByValue(data);
                break;
                
            case 3:
                list.display();
                break;
                
            case 4:
                cout << "Enter data to insert at front: ";
                cin >> data;
                list.insertAtFront(data);
                cout << "data inserted at front successfully" << endl;
                break;
                
            case 5:
                cout << "Number of elements: " << list.countElements() << endl;
                break;
                
            case 6:
                cout << "Enter node number: ";
                cin >> pos;
                list.printNthNode(pos);
                break;
                
            case 7:
                cout << "Enter data: ";
                cin >> data;
                cout << "Enter position: ";
                cin >> pos;
                list.insertAtPosition(data, pos);
                break;
                
            case 8:
                list.printLastNode();
                break;
                
            case 9:
                cout << "Enter data to insert (will keep sorted): ";
                cin >> data;
                list.insertSorted(data);
                cout << "data inserted in sorted order" << endl;
                break;
                
            case 10:
                list.reverse();
                break;
                
            case 11:
                list.sortList();
                break;
                
            case 12:
                cout << "Enter position to delete: ";
                cin >> pos;
                list.deleteByPosition(pos);
                break;
                
            case 0:
                cout << "exiting... grass touching time" << endl;
                break;
                
            default:
                cout << "invalid choice bestie, try again" << endl;
        }
    } while (choice != 0);
    
    return 0;
}


```
