#### 1.  Implement a method to perform basic string compressio using the counts of repeated characters. For example, the string aabcccccaaa would become a2b1c5a3. If the "compressed" string would not become smaller than the original string,your method should return the original string. [C or CPP]

```c
#include <stdio.h>
#include <assert.h>
#include <string.h>
#include <stdlib.h>

char* compress_string(char* str) {
    int len = strlen(str);
    if (len >= 2) {
        char result[len];
        int counter = 0;
        int resulti = 0;
        for (int i = 1; i <= len; i++) {
            char c = i < len ? str[i] : '\0';
            char p = str[i - 1];
            if (c == p) {
                counter++;
            } else {
                result[resulti++] = p;
                result[resulti++] = (char)(((int)'0') + counter + 1);
                counter = 0;
            }
        }
        result[resulti++] = '\0';
        int result_len = strlen(result);
        if (result_len < len) {
            char* ret = (char*)malloc(result_len + 1);
            memcpy(ret, result, result_len + 1);
            return ret;
        }
    }
    char* ret = (char*)malloc(len + 1);
    memcpy(ret, str, len + 1);
    ret[len] = '\0';
    return ret;
}

void test_compress_string() {
    char* result = compress_string("aabcccccaaa");
    assert(strcmp(result, "a2b1c5a3") == 0);
    printf("Passed!\n");
    free(result);
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
	size_t original_length = str.length();
	if (original_length < 2) {
		return str;
	}
	std::string out{""};
	int count = 1;
	for( size_t i = 1; i < original_length; ++i ) {
		if (str[i-1] == str[i]) {
			++count;
		} else {
			out += str[i-1];
			out += std::to_string(count);
			count = 1;
		}
		if (out.length() >= original_length) {
			return str;
		}
	}
	out += str[original_length-1];
	out += std::to_string(count);
	if (out.length() >= original_length) {
		return str;
	}
	return out;
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

#### 2. Rotate Matrix: Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?

```c
#include <stdio.h>
#include <assert.h>

void rotate_image_by_90deg(int **m, int N) {
    for (int layer = 0; layer < N / 2; ++layer) {
        int first = layer;
        int last = N - 1 - layer;

        for (int i = first; i < last; ++i) {
            int offset = i - first;
            int top = m[first][i];
            m[first][i] = m[last - offset][first];
            m[last - offset][first] = m[last][last - offset];
            m[last][last - offset] = m[i][last];
            m[i][last] = top;
        }
    }
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


### 3.  Remove Duplicates: Write code to remove duplicates from an unsorted single linked list.How would you solve this problem if a temporary buffer is not allowed?

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
	while(head) {
		std::cout << head->data << "-->";
		head = head->next;
	}
	std::cout << "nullptr" << std::endl;
}

//space complexity O(1)
//time complexity O(n^2)
void remove_duplicates(Node* head) {
	if (head == nullptr || (head && (head->next == nullptr)))
	{
		return;
	}
	Node * curr = head;
	while(curr) {
		Node * runner = curr;
		while (runner->next != nullptr)
		{
			if (runner->next->data == curr->data)
			{
				delete runner->next;
				runner->next = runner->next->next;
			}
			else
			{
				runner = runner->next;
			}
		}
		curr = curr->next;
	}
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

#### 5. Difference between mutexes and semaphores?
    * A mutex object enables one thread into a controlled section, forcing other threads which tries to gain access to that section to wait until the first thread has moved out from that section
    * Semaphore allows multiple access to shared resources
    
    * Mutex can only be released by thread which had acquired it
    * A semaphore can be signaled from any other thread or process.
    
    * Mutex will always have a known owner
    * While for semaphore you won’t know which thread we are blocking on
    
    * Mutex is also a tool that is used to provide deadlock-free mutual exclusion (either consumer or producer can have the key and proceed with their work)
    * Semaphore is a synchronization tool to overcome the critical section problem
    
    * Mutexes by definition are binary semaphores, so there are two states locked or unlocked
    * Semaphores are usually referred to counted locks

#### 6.  Set an integer variable at the absolute address 0x67EF to the value 0x88AA. The compiler is a pure ANSI compiler. Write code to accomplish this task.

```c
int main(void)
{
    //1
	int *ptr;
    ptr = (int *)0x67EF;
    *ptr = 0x88AA;

    //2
    *(int * const)(0x67EF) = 0x88AA;
	return 0;
}
```

#### 7. What does the keyword volatile mean? Give some examples of its use. 

A volatile variable is one that can change unexpectedly. Consequently, the compiler can make no assumptions about the value of the variable. In particular, the optimizer must be careful to reload the variable every time it is used instead of holding a copy in a register. Examples of volatile variables are: 
* Hardware registers in peripherals (e.g., status registers)
* Non-stack variables referenced within an interrupt service routine.
* Variables shared by multiple tasks in a multi-threaded application.

(a) Can a parameter be both const and volatile? Explain your answer.
(b) Can a pointer be volatile? Explain your answer.
(c) What is wrong with the following function?:

```c
int square(volatile int *ptr)
{
    return *ptr * *ptr;
}
``` 

The answers are as follows:

(a) Yes. An example is a read only status register. It is volatile because it can change unexpectedly. It is const because the program should not attempt to modify it.

(b) Yes. Although this is not very common. An example is when an interrupt service routine modifies a pointer to a buffer.

(c) The intent of the code is to return the square of the value pointed to by *ptr. However, since *ptr points to a volatile parameter, the compiler will generate code that looks something like this:

```c
int square(volatile int *ptr)
{
    int a,b;
    a = *ptr;
    b = *ptr;
    return a * b;
}
``` 
Since it is possible for the value of *ptr to change unexpectedly, it is possible for a and b to be different. Consequently, this code could return a number that is not a square! The correct way to code this is:

```c
long square(volatile int *ptr)
{
    int a;
    a = *ptr;
    return a * a;
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
The correct answer is that this code won’t even compile. The std::unique_ptr type cannot be copied, so passing it as a parameter to a function will fail to compile.

To convince the compiler that this is fine, std::move can be used:
ptr = f(std::move(ptr));
Follow-up questions
The interview candidate might think that returning a noncopiable object from a function is also a compiler error, but in this case it’s allowed, thanks to copy elision. You can ask the candidate under what conditions copy elision is performed.

Of course, the above construct with std::move is less than ideal. Ask the candidate how they would change the function f to make it better. For example, passing a (const) reference to the unique_ptr, or simply a reference to the int pointed to, is probably preferred.
