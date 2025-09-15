# CODE:

```

#include <iostream>
using namespace std;

class Node {
public:
    int data;
    Node *next;

    Node(int new_data) {
        data = new_data;
        next = nullptr;
    }
};

// Function to reverse the linked list
Node *reverseList(Node *head) {
    Node *curr = head, *prev = nullptr, *next;

    while (curr != nullptr) {
        next = curr->next;   // Store next
        curr->next = prev;   // Reverse current node's link
        prev = curr;         // Move prev forward
        curr = next;         // Move curr forward
    }
    return prev; // new head
}

// Function to print linked list
void printList(Node *node) {
    while (node != nullptr) {
        cout << node->data;
        if (node->next)
            cout << " -> ";
        node = node->next;
    }
    cout << endl;
}

int main() {
    int n, val;
    cout << "Enter number of nodes: ";
    cin >> n;

    if (n <= 0) {
        cout << "No nodes to create." << endl;
        return 0;
    }

    cout << "Enter elements: ";
    cin >> val;
    Node *head = new Node(val);
    Node *temp = head;

    // Build the linked list
    for (int i = 1; i < n; i++) {
        cin >> val;
        temp->next = new Node(val);
        temp = temp->next;
    }

    cout << "Original list: ";
    printList(head);

    head = reverseList(head);

    cout << "Reversed list: ";
    printList(head);

    return 0;
}


```

# OUTPUT : 

```
Enter number of nodes: 5
Enter elements: 2
4
8
1
3
Original list: 2 -> 4 -> 8 -> 1 -> 3
Reversed list: 3 -> 1 -> 8 -> 4 -> 2
```
