# C/CPP QUESTIONS

#### 1.  Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2b1c5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. [C/CPP]

```c
#include <stdio.h>
#include <assert.h>
#include <string.h>
#include <stdlib.h>

char* compress_string(char* str) {

    return NULL;
}

void test_compress_string() {
    char* compressed = compress_string("aabcccccaaa");
    assert(strcmp(compressed, "a2b1c5a3") == 0);
    printf("Passed!\n");
    //
}

int main(void) {
	test_compress_string();
	return 0;
}
```

```cpp
#include <iostream>
#include <string>
#include <cassert>

std::string compress_string(const std::string& str)
{
	return std::string("");
}

void test_compress_string() {
    std::string str =  "aabcccccaaa";
    std::string compressed = "a2b1c5a3";
    std::string result = compress_string(str);
    assert(result.compare(compressed) == 0);
    std::cout << "Passed!\n";
}

int main(void) {
	test_compress_string();
	return 0;
}
```

#### 2. Rotate Image: Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?

```c
#include <stdio.h>
#include <assert.h>

void rotate_image_by_90deg(int **m, int N) {

}

void test_rotate() {
	/*  BEFORE:
        int img[N][N] = {
        {0,  1,  2,   3},
        {4,  5,  6,   7},
        {8,  9,  10,  11},
        {12, 13, 14,  15},
    };
    */
    const int N = 4;
    int value = 0;
    int **img = new int *[10];
	for(int i = 0; i < N; i++)
	{
    	img[i] = new int[N];
		for(int j = 0; j < N; j++)
		{
			img[i][j] = value++;	
		}
	}

    rotate_image_by_90deg(img, N);
    
    assert(img[0][0] == 12);
    assert(img[1][1] == 9);
    assert(img[2][2] == 6);
    assert(img[3][3] == 3);

    printf("Passed!\n");
    //Clean
}

int main(void) {
	test_rotate();
	return 0;
}
```


#### 3.  Remove Duplicates: Write code to remove duplicates from an unsorted single linked list.How would you solve this problem if a temporary buffer is not allowed?

```cpp
#include <iostream>
#include <cassert>

struct Node {
	int data = 0;
	Node* next = nullptr;
};

void insert(Node*& head, int data)
{
	Node* newNode = new Node;
	newNode->data = data;
	newNode->next = head;
	head = newNode;
}

void print_node_list(Node* head) {
	// TODO
}

void remove_duplicates(Node* head) {
    //TODO
}

void test_removing_duplicates() {
    Node* head = nullptr;
    insert(head, 0);
    insert(head, 1);
    insert(head, 2);
    insert(head, 2);
    insert(head, 2);
    insert(head, 3);
    insert(head, 3);

    remove_duplicates(head);
    print_node_list(head);
    assert(head->data == 3);
    assert(head->next->data == 2);
    assert(head->next->next->data == 1);
    assert(head->next->next->next->data == 0);
    printf("Passed!\n");

}

int main(void) {
	test_removing_duplicates();
	return 0;
}
```


#### 4. What is semaphore, mutex, spinlock, critical section?

#### 5. Differences between mutexes and semaphores?

#### 6.  Set an integer variable at the absolute address 0x67EF to the value 0x88AA. The compiler is a pure ANSI compiler. Write code to accomplish this task.

```c
int main(void)
{
    // TODO
	return 0;
}
```


#### 7. What does the keyword volatile mean? Give some examples of its use. 

* What is wrong with the following function?:
```c
int square(volatile int *ptr)
{
    return *ptr * *ptr;
}
``` 


#### 8. Is there anything wrong with this code snippet ?
```cpp
#include <memory>

auto foo(std::unique_ptr<int> ptr) {
  *ptr = 42;
  return ptr;
}

int main() {
  auto ptr = std::make_unique<int>();
  ptr = foo(ptr);
}
```
