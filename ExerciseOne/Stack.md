# STACK - FILO


# Code

```
#include <iostream>
using namespace std;

struct node {
    int data;
    struct node* link; // self referencing, point to another node of the same type
};

class Stack {
    struct node* top;   // pointer to top of stack

public:
    Stack() {
        top = nullptr;
    }

    // PUSH
    void push(int x) {
        struct node* temp = new node();
        temp->data = x;
        temp->link = top; //new node point at the current top of the stack, if top = null, temp->link is null
        top = temp; //top -> 20|null becomes top -> 30\link -> 20\null
    }

    // POP
    void pop() {
        if (top == nullptr) {
            cout << "Stack Underflow! Cannot pop.\n";
            return;
        }
        struct node* temp = top;
        cout << "Popped element: " << temp->data << endl;
        top = top->link; //move top to the next node
        delete temp;
    }

    // PEEK
    void peek() {
        if (top == nullptr) {
            cout << "Stack is empty!\n";
        } else {
            cout << "Top element: " << top->data << endl;
        }
    }

    // DISPLAY
    void display() {
        if (top == nullptr) {
            cout << "Stack is empty!\n";
            return;
        }
        struct node* temp = top;
        cout << "Stack elements: ";
        while (temp != nullptr) {
            cout << temp->data << " ";
            temp = temp->link;
        }
        cout << endl;
    }
};

int main() {
    Stack st;
    int choice, x;

    do {
        cout << "\n--- Stack Menu ---\n";
        cout << "1. Push\n";
        cout << "2. Pop\n";
        cout << "3. Peek\n";
        cout << "4. Display\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter value to push: ";
            cin >> x;
            st.push(x);
            break;
        case 2:
            st.pop();
            break;
        case 3:
            st.peek();
            break;
        case 4:
            st.display();
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

# Output

```
--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 1
Enter value to push: 2

--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 1
Enter value to push: 3

--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 1
Enter value to push: 4

--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 1
Enter value to push: 5

--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 1
Enter value to push: 6

--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 1
Enter value to push: 7

--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 2
Popped element: 7

--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 3
Top element: 6

--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 4
Stack elements: 6 5 4 3 2 

--- Stack Menu ---
1. Push
2. Pop
3. Peek
4. Display
5. Exit
Enter your choice: 5
Exiting...


```
