# QUEUE - FIFO

# CODE

```

#include <iostream>
using namespace std;

// Node structure
struct node {
    int data;
    struct node* link;
};

class Queue {
    struct node* front; // points to first element
    struct node* rear;  // points to last element

public:
    
    Queue() { 
        front = nullptr;
        rear = nullptr;
    }

    // ENQUEUE 
    void enqueue(int x) {
        struct node* temp = new node();
        temp->data = x;
        temp->link = nullptr;

        if (rear == nullptr) { // if queue is empty
            front = rear = temp;
        } else {
            rear->link = temp; //the previous node points to the new node
            rear = temp; //rear points to the new node
        }
        cout << x << " enqueued into queue.\n";
    }

    // DEQUEUE (Remove from front)
    void dequeue() {
        if (front == nullptr) {
            cout << "Queue Underflow! Cannot dequeue.\n";
            return;
        }
        struct node* temp = front; // front and temp points the same node 
        cout << "Dequeued element: " << temp->data << endl;
        front = front->link; // front-> link -> link == front

        if (front == nullptr) { // queue became empty
            rear = nullptr;
        }
        delete temp;
    }

    // PEEK (Front element)
    void peek() {
        if (front == nullptr) {
            cout << "Queue is empty!\n";
        } else {
            cout << "Front element: " << front->data << endl;
        }
    }

    // DISPLAY
    void display() {
        if (front == nullptr) {
            cout << "Queue is empty!\n";
            return;
        }
        struct node* temp = front;
        cout << "Queue elements: ";
        while (temp != nullptr) {
            cout << temp->data << " ";
            temp = temp->link;
        }
        cout << endl;
    }
};

// Driver Code
int main() {
    Queue q;
    int choice, x;

    do {
        cout << "\n--- Queue Menu ---\n";
        cout << "1. Enqueue\n";
        cout << "2. Dequeue\n";
        cout << "3. Peek\n";
        cout << "4. Display\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter value to enqueue: ";
            cin >> x;
            q.enqueue(x);
            break;
        case 2:
            q.dequeue();
            break;
        case 3:
            q.peek();
            break;
        case 4:
            q.display();
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

# OUTPUT


```
#include <iostream>
using namespace std;

// Node structure
struct node {
    int data;
    struct node* link;
};

class Queue {
    struct node* front; // points to first element
    struct node* rear;  // points to last element

public:
    
    Queue() { 
        front = nullptr;
        rear = nullptr;
    }

    // ENQUEUE 
    void enqueue(int x) {
        struct node* temp = new node();
        temp->data = x;
        temp->link = nullptr;

        if (rear == nullptr) { // if queue is empty
            front = rear = temp;
        } else {
            rear->link = temp; //the previous node points to the new node
            rear = temp; //rear points to the new node
        }
        cout << x << " enqueued into queue.\n";
    }

    // DEQUEUE (Remove from front)
    void dequeue() {
        if (front == nullptr) {
            cout << "Queue Underflow! Cannot dequeue.\n";
            return;
        }
        struct node* temp = front; // front and temp points the same node 
        cout << "Dequeued element: " << temp->data << endl;
        front = front->link; // front-> link -> link == front

        if (front == nullptr) { // queue became empty
            rear = nullptr;
        }
        delete temp;
    }

    // PEEK (Front element)
    void peek() {
        if (front == nullptr) {
            cout << "Queue is empty!\n";
        } else {
            cout << "Front element: " << front->data << endl;
        }
    }

    // DISPLAY
    void display() {
        if (front == nullptr) {
            cout << "Queue is empty!\n";
            return;
        }
        struct node* temp = front;
        cout << "Queue elements: ";
        while (temp != nullptr) {
            cout << temp->data << " ";
            temp = temp->link;
        }
        cout << endl;
    }
};

// Driver Code
int main() {
    Queue q;
    int choice, x;

    do {
        cout << "\n--- Queue Menu ---\n";
        cout << "1. Enqueue\n";
        cout << "2. Dequeue\n";
        cout << "3. Peek\n";
        cout << "4. Display\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter value to enqueue: ";
            cin >> x;
            q.enqueue(x);
            break;
        case 2:
            q.dequeue();
            break;
        case 3:
            q.peek();
            break;
        case 4:
            q.display();
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

````
