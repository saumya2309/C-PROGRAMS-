# C-PROGRAMS-


#### write a cpp program to implement a singly linked list using a struct node and dynamic memory allocation   with new and delete . Implement the following operations :
#### insertion of a node at the beginning  
#### insertio of a node at the end
#### deletion of a node
#### display the entire list
#### use new to allocate memory for each node , and delete to free memory when deleting nodes

```c
#include <iostream>
using namespace std;

struct node {
    int data;
    node* next;
};

node* head = NULL;

// Insert at beginning
void insertBeg(int val) {
    node* n = new node{val, head};
    head = n;
}

// Insert at end
void insertEnd(int val) {
    node* n = new node{val, NULL};
    if (!head) { head = n; return; }

    node* temp = head;
    while (temp->next)
        temp = temp->next;

    temp->next = n;
}

// Delete a node by value
void deleteNode(int val) {
    if (!head) return;

    if (head->data == val) {
        node* temp = head;
        head = head->next;
        delete temp;
        return;
    }

    node* temp = head;
    while (temp->next && temp->next->data != val)
        temp = temp->next;

    if (temp->next) {
        node* del = temp->next;
        temp->next = del->next;
        delete del;
    }
}

// Display list
void display() {
    node* temp = head;
    while (temp) {
        cout << temp->data << " -> ";
        temp = temp->next;
    }
    cout << "NULL\n";
}

int main() {
    insertBeg(10);
    insertEnd(20);
    insertEnd(30);
    display();

    deleteNode(20);
    display();

    return 0;
}
```

<img width="310" height="86" alt="image" src="https://github.com/user-attachments/assets/6cb2cf37-cb2d-457e-9ff4-be3dac365406" />


####  write a c++ program that dynamically allocates memory for an array of integers using new based on the size input by the user. the program should: 
#### allow the user to enter the size of the array 
#### allow the user to enter the values of array
#### print the array
#### free the dynamically alloacted memory using delete

```c
#include <iostream>
using namespace std;

int main() {
    int size;

    cout << "Enter size of the array: ";
    cin >> size;

    // Dynamically allocate array
    int* arr = new int[size];

    // Input values
    cout << "Enter " << size << " integers:\n";
    for (int i = 0; i < size; i++) {
        cin >> arr[i];
    }

    // Print the array
    cout << "Array elements are: ";
    for (int i = 0; i < size; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    // Free memory
    delete[] arr;

    return 0;
}
```

<img width="502" height="159" alt="image" src="https://github.com/user-attachments/assets/066365dc-95c4-4ea7-9b3a-a4703a7266ff" />

```
```
#### swap two numbers using pointers 
#### write a c++ program that swaps two numbers using pointers . the program should
#### declare two integer variables
#### use a pointer to swap their values
#### print the swapped values

```c

#include <iostream>
using namespace std;

void swap(int* a, int* b) {
    int temp = *a;  // store value pointed by a
    *a = *b;        // assign value pointed by b to a
    *b = temp;      // assign temp to b
}

int main() {
    int x, y;
    cout << "Enter two integers: ";
    cin >> x >> y;

    cout << "Before swap: x = " << x << ", y = " << y << endl;

    swap(&x, &y);  // pass addresses to swap function

    cout << "After swap: x = " << x << ", y = " << y << endl;

    return 0;
}
```
<img width="531" height="147" alt="image" src="https://github.com/user-attachments/assets/ae46e147-2ecc-4ee5-82b9-02efda7cabeb" />

#### Write a c++ program that dynamically allocates memory for an array of strings ( an array of pointers). The program should :
#### Allow the user to input multiple strings.
#### Print all the strings using the array of pointers.
#### Free the allocated memory for each string and the array of pointers using delete.

```c
#include <iostream>
#include <cstring>
using namespace std;

int main() {
    int n;
    cout << "Number of strings: ";
    cin >> n;
    cin.ignore();

    char** arr = new char*[n];

    for (int i = 0; i < n; i++) {
        char temp[1000];
        cout << "String " << i + 1 << ": ";
        cin.getline(temp, 1000);
        arr[i] = new char[strlen(temp) + 1];
        strcpy(arr[i], temp);
    }

    cout << "\nStrings entered:\n";
    for (int i = 0; i < n; i++)
        cout << arr[i] << endl;

    for (int i = 0; i < n; i++)
        delete[] arr[i];
    delete[] arr;

    return 0;
}
```


<img width="277" height="320" alt="image" src="https://github.com/user-attachments/assets/814df781-f504-4bc9-b7df-cea26665cd17" />

```
```
#### Write a C++ program that implements a circular buffer using a dynamically allocated array. The program should :

#### Dynamically  allocate a memory for the buffer
#### Allow the user to add and remove elements
#### Handle overflow and underflow condition
#### Properly deallocate the memory used by the buffer

```c
#include <iostream>
using namespace std;

class CircularBuffer {
    int* buffer;
    int capacity;
    int front, rear, count;

public:
    CircularBuffer(int size) {
        capacity = size;
        buffer = new int[capacity];
        front = 0;
        rear = -1;
        count = 0;
    }

    ~CircularBuffer() {
        delete[] buffer;  // free memory
    }

    void add(int value) {
        if (count == capacity) {
            cout << "Overflow! Buffer is full.\n";
            return;
        }
        rear = (rear + 1) % capacity;
        buffer[rear] = value;
        count++;
        cout << value << " added.\n";
    }

    void remove() {
        if (count == 0) {
            cout << "Underflow! Buffer is empty.\n";
            return;
        }
        cout << buffer[front] << " removed.\n";
        front = (front + 1) % capacity;
        count--;
    }

    void display() {
        if (count == 0) {
            cout << "Buffer is empty.\n";
            return;
        }
        cout << "Buffer elements: ";
        for (int i = 0; i < count; i++)
            cout << buffer[(front + i) % capacity] << " ";
        cout << endl;
    }
};

int main() {
    int size, choice, value;
    cout << "Enter buffer size: ";
    cin >> size;

    CircularBuffer cb(size);

    do {
        cout << "\n1. Add\n2. Remove\n3. Display\n4. Exit\nChoice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value to add: ";
                cin >> value;
                cb.add(value);
                break;
            case 2:
                cb.remove();
                break;
            case 3:
                cb.display();
                break;
            case 4:
                cout << "Exiting...\n";
                break;
            default:
                cout << "Invalid choice!\n";
        }
    } while (choice != 4);

    return 0;
}
```

<img width="468" height="707" alt="image" src="https://github.com/user-attachments/assets/320f021b-3289-43ec-a23e-ff3239736078" />

<img width="979" height="168" alt="image" src="https://github.com/user-attachments/assets/24665914-9c56-4a77-b15f-beba3288e3b8" />


```
```
#### Write a C++ program that demonstrates the use of a pointer to a constant variable . The program should :

#### Declare a constant variable and a pointer to it.
#### Show how you can read the value pointed to by the pointer ,
#### but not modify it.

```c
#include <iostream>
using namespace std;

int main() {

    const int number = 100;

   
    const int* ptr = &number;


    cout << "Value of number: " << number << endl;
    cout << "Value pointed to by ptr: " << *ptr << endl;

   
}

```

<img width="297" height="103" alt="image" src="https://github.com/user-attachments/assets/4af61b74-a062-48a1-9fef-448b060e7f01" />

```
```
#### Write a C++ program where a function returns a reference to a local variable. What are potential problems and how to #### avoid them. Implement a version where the function returns a refernce to a static or globally declared variable.
```c
#include <iostream>
using namespace std;

int& getValue() {
    int x = 10;   
    return x;    
}

int main() {
    int& ref = getValue();  
    cout << "Value: " << ref << endl;  

    ref = 50;  // Attempt to modify invalid memory
    cout << "Modified Value: " << ref << endl;

    return 0;
}
```
