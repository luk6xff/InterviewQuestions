1.  Implement a method to perform basic string compression
    using the counts of repeated characters. For example, the
    string aabcccccaaa would become a2b1c5a3. If the "compressed"
    string would not become smaller than the original string,
    your method should return the original string. [C]

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

//-----------------------------------------------------------------------------
2. Rotate Matrix: Given an image represented by an NxN matrix, where each pixel in the image is 4
bytes, write a method to rotate the image by 90 degrees. Can you do this in place?

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


//-----------------------------------------------------------------------------
3.  Remove Duplicates: Write code to remove duplicates from an unsorted single linked list.
    How would you solve this problem if a temporary buffer is not allowed?

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