```cpp
#include <iostream>
using namespace std;

class Node {
public:
    union Value {
        int num;
        char ch;
        float decimal;
    } val;
    Node* next;
    Node* prev;
    
    Node() {
        next = nullptr;
        prev = nullptr;
    }
};

class LinkedList {
protected:
    int len;
    Node* head;
public:
    typedef Node::Value ListValue;
    
    LinkedList() {
        head = nullptr;
        len = 0;
    }
    
    ~LinkedList() {
        Node* current = new Node();
        current = head;
        
        while (current != nullptr) {
            Node* temp = current->next;
            delete current;
            current = temp;
        }
        cout << "Linked List is deleted\n";
    }
    
    void addNode(ListValue input, int pos) {
        if (pos > len || pos < 0) {
            cout << "Invalid Node addition at position " << pos << "\n";
            return;
        }
        
        Node* newNode = new Node();
        newNode->val = input;
        
        if (pos == 0) {
            newNode->next = head;
            if (head != nullptr) {
                head->prev = newNode;
            }
            head = newNode;
        } else {
            Node* current = new Node();
            current = head;
            for (int i = 0; i < pos - 1; i++) {
                current = current->next;
            }
            newNode->next = current->next;
            newNode->prev = current;
            if (current->next != nullptr) {
                current->next->prev = newNode;
            }
            current->next = newNode;
        }
        len++;
    }
    
    void deleteNode(int pos)
    {
        if (pos>len-1)
        {
            cout << "Incorrect deletion of non existent node" <<endl;
            return;
        }
        
        Node* current = new Node();
        current = head;
        
        int countr=pos;
        while(countr>0)
        {
            current = current->next;
            countr-=1;
        }
        
        if (pos<len-1)
        {
            current->next->prev = current->prev;
            
            if(pos==0)
            {
                head = current->next;
                len-=1;
            }
        }
        
        if (pos<=len-1 && pos!=0)
        {
            current->prev->next=current->next;
            len-=1;
        }
        
        delete current;
    }
    
    void printList() {
        Node* current = new Node();
        current = head;
        while (current != nullptr) {
            cout << current->val.num << " <--> ";
            current = current->next;
        }
        cout<<"NULL" << endl <<endl;
        
        delete current;
    }
};

int main() {
    int pos;
    LinkedList* list = new LinkedList();
    LinkedList::ListValue input;
    
    cout << "Enter values to form new Node: " <<endl;
    for (int i = 0; i < 5; i++) {
        cin >> input.num;
        list->addNode(input, i);
        list->printList();
    }
    
    cout<<"Enter position for deleting Node: " << endl;
    cin >> pos;
    list->deleteNode(pos);
    list->printList();
    
    cout<<"Enter position for deleting Node: " <<endl;
    cin >> pos;
    list->deleteNode(pos);
    list->printList();
    
    delete list;
    return 0;
}
```


