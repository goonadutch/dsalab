# CODE :

```

#include <iostream>
using namespace std;

// Node structure
class Node {
public:
    int data;
    Node *next;

    Node(int value) {
        data = value;
        next = nullptr;
    }
};

class CircularQueue {
    Node *rear; // rear->next will be the front

public:
    CircularQueue() {
        rear = nullptr;
    }

    // ENQUEUE
    void enqueue(int value) {
        Node *temp = new Node(value);

        if (rear == nullptr) { 
            rear = temp;
            rear->next = rear; // circular link
        } else {
            temp->next = rear->next; // new node points to front
            rear->next = temp;       // old rear points to new node
            rear = temp;             // rear updated
        }
        cout << value << " enqueued.\n";
    }

    // DEQUEUE
    void dequeue() {
        if (rear == nullptr) {
            cout << "Queue Underflow! Cannot dequeue.\n";
            return;
        }

        Node *front = rear->next; // front = next of rear

        if (rear == front) { // only one node
            cout << "Dequeued: " << front->data << endl;
            delete front;
            rear = nullptr;
        } else {
            cout << "Dequeued: " << front->data << endl;
            rear->next = front->next; // skip the front node
            delete front;
        }
    }

    // PEEK (front element)
    void peek() {
        if (rear == nullptr) {
            cout << "Queue is empty!\n";
        } else {
            cout << "Front element: " << rear->next->data << endl;
        }
    }

    // DISPLAY
    void display() {
        if (rear == nullptr) {
            cout << "Queue is empty!\n";
            return;
        }
        Node *temp = rear->next; // start from front
        cout << "Queue elements: ";
        do {
            cout << temp->data << " ";
            temp = temp->next;
        } while (temp != rear->next);
        cout << endl;
    }
};

// Driver Code
int main() {
    CircularQueue cq;
    int choice, x;

    do {
        cout << "\n--- Dynamic Circular Queue Menu ---\n";
        cout << "1. Enqueue\n";
        cout << "2. Dequeue\n";
        cout << "3. Peek\n";
        cout << "4. Display\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter value: ";
            cin >> x;
            cq.enqueue(x);
            break;
        case 2:
            cq.dequeue();
            break;
        case 3:
            cq.peek();
            break;
        case 4:
            cq.display();
            break;
        case 5:
            cout << "Exiting...\n";
            break;
        default:
            cout << "Invalid choice! Try again.\n";
        }
    } while (choice != 5);

    return 0;
}


```


