---
layout: post
title: Implementing a singly linked list in C++.
comments: true
tags:
  - Linked List
  - Data structure
  - C++
---

In this post, I will try to implement a singly linked list using C++.

# Linked List
> In computer science, a linked list is a linear collection of data elements, called nodes pointing to the next node by means of a pointer. It is a data structure consisting of a group of nodes which together represent a sequence. Under the simplest form, each node is composed of data and a reference (in other words, a link) to the next node in the sequence; more complex variants add additional links. [^1]

There are various kinds of linked lists such as singly linked list, doubly linked list, circular linked list etc.

A node of a singly linked list containing integer data is as shown below.

```c++
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

```c++
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
```

Inserting a node at the head is trivial. But while inserting a node at the end, we will have to move our pointer to the last element of the list. Then we can insert a new node after that element and point it's next pointer to NULL.

```c++
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

In case of search functionality, we will have to traverse the list from head to the last element untill we find a match.

```c++
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

That's it.

* __The average time for searching an element is `O(n)`__.
* __The average time of inserting an element at the head is `O(1)`__.
* __The average time for removing an element from the head is `O(1)`__.

[^1]: [https://en.wikipedia.org/wiki/Linked_list](https://en.wikipedia.org/wiki/Linked_list)
