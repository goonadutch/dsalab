# CODE :

```

#include <iostream>
using namespace std;

// Node structure
class Node {
public:
    int data;
    Node* next;

    Node(int value) {
        data = value;
        next = nullptr;
    }
};

class SinglyLinkedList {
    Node* head;

public:
    SinglyLinkedList() {
        head = nullptr;
    }

    // INSERT at end
    void insert(int value) {
        Node* newNode = new Node(value);
        if (head == nullptr) {
            head = newNode;
        } else {
            Node* temp = head;
            while (temp->next != nullptr) {
                temp = temp->next;
            }
            temp->next = newNode;
        }
        cout << value << " inserted.\n";
    }

    // DELETE by value
    void remove(int value) {
        if (head == nullptr) {
            cout << "List is empty!\n";
            return;
        }

        // If head node holds the value
        if (head->data == value) {
            Node* temp = head;
            head = head->next;
            delete temp;
            cout << value << " deleted.\n";
            return;
        }

        Node* temp = head;
        while (temp->next != nullptr && temp->next->data != value) {
            temp = temp->next;
        }

        if (temp->next == nullptr) {
            cout << value << " not found in list!\n";
        } else {
            Node* delNode = temp->next;
            temp->next = temp->next->next;
            delete delNode;
            cout << value << " deleted.\n";
        }
    }

    // DISPLAY
    void display() {
        if (head == nullptr) {
            cout << "List is empty!\n";
            return;
        }
        cout << "Linked List: ";
        Node* temp = head;
        while (temp != nullptr) {
            cout << temp->data;
            if (temp->next != nullptr)
                cout << " -> ";
            temp = temp->next;
        }
        cout << endl;
    }
};

// Driver code
int main() {
    SinglyLinkedList list;
    int choice, value;

    do {
        cout << "\n--- Singly Linked List Menu ---\n";
        cout << "1. Insert\n";
        cout << "2. Delete\n";
        cout << "3. Display\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter value to insert: ";
            cin >> value;
            list.insert(value);
            break;
        case 2:
            cout << "Enter value to delete: ";
            cin >> value;
            list.remove(value);
            break;
        case 3:
            list.display();
            break;
        case 4:
            cout << "Exiting...\n";
            break;
        default:
            cout << "Invalid choice! Try again.\n";
        }
    } while (choice != 4);

    return 0;
}


```

# OUTPUT :

```

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 1
Enter value to insert: 34
34 inserted.

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 1
Enter value to insert: 2
2 inserted.

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 1
Enter value to insert: 56
56 inserted.

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 1
Enter value to insert: 4
4 inserted.

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 1
Enter value to insert: 8
8 inserted.

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 1
Enter value to insert: 7
7 inserted.

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 1
Enter value to insert: 3
3 inserted.

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 3
Linked List: 34 -> 2 -> 56 -> 4 -> 8 -> 7 -> 3

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 2
Enter value to delete: 2
2 deleted.

--- Singly Linked List Menu ---
1. Insert
2. Delete
3. Display
4. Exit
Enter your choice: 3
Linked List: 34 -> 56 -> 4 -> 8 -> 7 -> 3

```
