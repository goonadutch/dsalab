#include <iostream>
using namespace std;

class Queue {
private:
    int front, rear, size;
    int* arr;

public:
    Queue(int capacity) {
        size = capacity;
        arr = new int[size];
        front = -1;
        rear = -1;
    }

    ~Queue() {
        delete[] arr;
    }


    void enqueue(int value) {
        if (rear == size - 1) {
            cout << "Queue Overflow\n";
            return;
        }
        if (front == -1) front = 0; 
        arr[++rear] = value;
        cout << value << " enqueued to queue\n";
    }


    void dequeue() {
        if (front == -1 || front > rear) {
            cout << "Queue Underflow\n";
            return;
        }
        cout << arr[front] << " dequeued from queue\n";
        front++;
    }


    int peek() {
        if (front == -1 || front > rear) {
            cout << "Queue is empty\n";
            return -1;
        }
        return arr[front];
    }


    bool isEmpty() {
        return (front == -1 || front > rear);
    }

    bool isFull() {
        return (rear == size - 1);
    }

    void display() {
        if (isEmpty()) {
            cout << "Queue is empty!\n";
            return;
        }
        cout << "Queue elements: ";
        for (int i = front; i <= rear; i++) {
            cout << arr[i] << " ";
        }
        cout << endl;
    }
};

int main() {
    int capacity;
    cout << "Enter the capacity of the queue: ";
    cin >> capacity;
    Queue q(capacity);

    int choice, value;

    do {
        cout << "\n--- Queue Menu ---\n";
        cout << "1. Enqueue\n";
        cout << "2. Dequeue\n";
        cout << "3. Peek\n";
        cout << "4. Check if Queue is Empty\n";
        cout << "5. Check if Queue is Full\n";
        cout << "6. Display Queue\n";
        cout << "7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter value to enqueue: ";
            cin >> value;
            q.enqueue(value);
            break;
        case 2:
            q.dequeue();
            break;
        case 3:
            cout << "Front element is: " << q.peek() << endl;
            break;
        case 4:
            if (q.isEmpty()) {
                cout << "The queue is empty.\n";
            } else {
                cout << "The queue is not empty.\n";
            }
            break;
        case 5:      
            if (q.isFull()) {
                cout << "The stack is full.\n";
            } else {
                cout << "The stack is not full.\n";
            }
            break;
        case 6:
            q.display();
        case 7:
            cout << "Exiting...\n";
            break;
        default:
            cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 6); 
    return 0;
}




------------------------------------------------------
-------------------------------------------------------





#include <iostream>
using namespace std;


class Stack {
    int* arr;
    int top;
    int capacity;

public:
    Stack(int size) {
        arr = new int[size]; 
        capacity = size;
        top = -1; 
    }

   
    ~Stack() {
        delete[] arr;
    }

    
    void push(int x) {
       
        if (isFull()) {
            cout << "Overflow! Cannot push " << x << "\n";
            return;
        }
        
        arr[++top] = x;
        cout << "Pushed " << x << " to the stack\n";
    }

   
    int pop() {
        
        if (isEmpty()) {
            cout << "Underflow! The stack is empty\n";
            return -1;
        }
        
        return arr[top--];
    }

    
    int peek() {
        if (!isEmpty()) {
            return arr[top];
        }
        else {
            return -1;
        }
    }

    
    bool isEmpty() {
        return top == -1;
    }

   
    bool isFull() {
        return top == capacity - 1;
    }

    void display() {
        if (isEmpty()) {
            cout << "Stack is empty!\n";
            return;
        }
        cout << "Stack elements: ";
        for (int i = top; i >= 0; i--) {
            cout << arr[i] << " ";
        }
        cout << endl;
    }

};

// Main function
int main() {
    int size;
    cout << "Enter the size of the stack: ";
    cin >> size;

    Stack stack(size);
    int choice, value;

    do {
        
        cout << "\n--- Stack Menu ---\n";
        cout << "1. Push\n";
        cout << "2. Pop\n";
        cout << "3. Peek\n";
        cout << "4. Check if the stack is empty\n";
        cout << "5. Check if the stack is full\n";
        cout << "6. Display the stack \n";
        cout << "7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
           
            cout << "Enter value to push: ";
            cin >> value;
            stack.push(value);
            break;
        case 2:
            
            value = stack.pop();
            if (value != -1) {
                cout << "Popped value: " << value << endl;
            }
            break;
        case 3:
            
            value = stack.peek();
            if (value != -1) {
                cout << "Top element: " << value << endl;
            } else {
                cout << "Stack is empty!" << endl;
            }
            break;
        case 4:
           
            if (stack.isEmpty()) {
                cout << "The stack is empty.\n";
            } else {
                cout << "The stack is not empty.\n";
            }
            break;
        case 5:
            
            if (stack.isFull()) {
                cout << "The stack is full.\n";
            } else {
                cout << "The stack is not full.\n";
            }
            break;
        case 6:
            stack.display();
        case 7:
            cout << "Exiting...\n";
            break;
        default:
            cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 7); 
    return 0;
}


-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------

#include <iostream>
using namespace std;

class CircularQueue {
private:
    int *queue;
    int front, rear, size, capacity;

public:
    CircularQueue(int capacity) {
        this->capacity = capacity;
        queue = new int[capacity];
        front = -1;
        rear = -1;
        size = 0;
    }

    ~CircularQueue() {
        delete[] queue;
    }

    bool isFull() {
        return size == capacity;
    }

    bool isEmpty() {
        return size == 0;
    }

    void enqueue(int value) {
        if (isFull()) {
            cout << "Queue is full. Cannot enqueue " << value << endl;
            return;
        }
        if (isEmpty()) {
            front = 0;
        }
        rear = (rear + 1) % capacity;
        queue[rear] = value;
        size++;
        cout << value << " enqueued to the queue." << endl;
    }

    void dequeue() {
        if (isEmpty()) {
            cout << "Queue is empty. Cannot dequeue." << endl;
            return;
        }
        cout << queue[front] << " dequeued from the queue." << endl;
        front = (front + 1) % capacity;
        size--;
        if (isEmpty()) {
            front = -1;
            rear = -1;
        }
    }

    void display() {
        if (isEmpty()) {
            cout << "Queue is empty." << endl;
            return;
        }
        cout << "Queue elements: ";
        for (int i = 0; i < size; i++) {
            cout << queue[(front + i) % capacity] << " ";
        }
        cout << endl;
    }
};

int main() {
    CircularQueue cq(5);

    cq.enqueue(10);
    cq.enqueue(20);
    cq.enqueue(30);
    cq.enqueue(40);
    cq.enqueue(50);

    cq.display();

    cq.dequeue();
    cq.dequeue();

    cq.display();

    cq.enqueue(60);
    cq.enqueue(70);

    cq.display();

    return 0;
}
