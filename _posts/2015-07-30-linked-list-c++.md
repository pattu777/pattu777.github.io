---
layout: post
title: Implementing a singly linked list in C++.
comments: true
---

In this post, I will try to implement a singly linked list using C++.

Linked List:

It's a kind of data structure where information is stored in a node and the nodes are grouped together in a sequence. Typically a node consists of a data and reference part where the reference pointer contains the address of a different node.

There are various kinds of linked lists such as singly linked list, doubly linked list, circular linked list etc.

A node of a singly linked list containing integers is as shown below.

```
---------------
| Data | next |
---------------  
struct Node
{
    int data;
    Node* next;
};
```

Now let's define a linked list class and implement some common functionalities.

```
class LinkedList
{
private:
    Node* head;
public:
    LinkedList();                        
    ~LinkedList();                      
    void display();
    void search(int);
    void insert_at_head(int);               
    void insert_after_node(Node*, int);   
    void insert_at_tail(int);              
};

LinkedList :: LinkedList()
{
    head = NULL;
}

LinkedList :: ~LinkedList()
{
}

// Add a node at the head.
void LinkedList :: insert_at_head(int num)
{
    Node* tmp = new Node;
    tmp->data = num;
    tmp->next = head;
    head = tmp;
}

// Insert a node after a certain node.
void LinkedList :: insert_after_node(Node* ptr, int num)
{
    if(head == NULL)
    {
        head = new Node;
        head->data = num;
        head->next = NULL;
    }
    else
    {
        Node* tmp = head;
        while((tmp->next != NULL) && (tmp->data != ptr->data))
            tmp = tmp->next;
        Node* current = new Node;
        current->data = num;
        current->next = tmp->next;
        tmp->next = current;
    }
}

// Insert a node at the end of the list.
void LinkedList :: insert_at_tail(int num)
{
    if(head == NULL)
    {
        head = new Node;
        head->data = num;
        head->next = NULL;
    }
    else
    {
        Node* tmp = head;
        while(tmp->next != NULL)
            tmp = tmp->next;
        Node* new_node = new Node;
        new_node->data = num
        new_node->next = NULL;
        tmp->next = new_node;
    }
}
```

Now let's finish off the display and search function.

```
void LinkedList :: display()
{
    if(head == NULL)
    {
        std::cout << "The linked list is empty" << std::endl;
        return;
    }
    else
    {
        Node* tmp = head;
        while(tmp != NULL)
        {
            std::cout << tmp->data << " ";
            tmp = tmp->next;
        }
        std::cout << std::endl;
    }
}

void LinkedList :: search(int num)
{
    if(head == NULL)
    {
        std::cout << "The linked list is empty" << std::endl;
        return;
    }
    else
    {
        Node* tmp = head;
        while(tmp != NULL)
        {
            if(tmp->data == num)
            {
                std::cout << "FOUND" << std::endl;
                return;
            }
            tmp = tmp->next;
        }
        std::cout << "NOT FOUND" << std::endl;
    }
}
```

That's it. The average time of insertion, deletion and search in a singly linked list is O(n).
