# <p align=‘centre’>Lab 1</p>

Variant 25: Queue with priority. Inserting an item into the queue. Taking an item from the queue.

## <p align=‘centre’>Purposes of the lab work:</p>
1. Develop a library for working with a queue with priority in the chosen imperative programming language (e.g. C++, Java, Python).
2. Create a test programme to demonstrate the functionality of the developed library.
3. Develop a system of tests to verify the performance and correctness of the library, taking into account the requirements of completeness, adequacy and consistency.
4. To provide processing of incorrect data, providing for correct termination of the programme in case of errors.
5. Write a report on the laboratory work.
   
## <p align=‘centre’>Tasks of the laboratory work:</p>
1. Study the specification of the task of working with the queue with priority.
2. Choose a programming language to implement the library (e.g. C++, Java, Python) according to the individual task.
3. Design and implement a library for working with a queue with priority, including operations of insertion and extraction of elements.
4. Write a test programme that demonstrates the basic scenarios of using the library.
5. Develop a test system including test cases to test various aspects of the library including correctness, performance and error handling.
6. Conduct testing of the developed library, ensuring that it works correctly on various inputs.
7. Write a detailed report, including a description of the problem solution, library architecture, testing results and conclusions.

## <p align=‘centre’>List of concepts and algorithms used:</p>
1. **Software Library (Code Library):**
In programming, a library is a collection of programme code intended for solving certain tasks. This code may contain functions, classes, procedures, or other components that developers can reuse in their software projects. Libraries simplify development because they provide ready-made solutions to typical problems.

2. **Priority Queue:**
   A priority queue is an abstract data structure like a stack or a queue, where each element has a priority. An element with a higher priority comes before an element with a lower priority.

3. ** Binary Heap:** Binary Heap is an abstract data structure like a stack or a queue where each element has a priority before the element with a higher priority.
   A binary heap is a type of binary tree that fulfils the following three conditions:
   - **Sorting of values:** The value at each node is not less than the values of its descendants. In the case of a min-heap, the value at each vertex is not greater than the values of its descendants; in a max-heap, it is not less.
   - **Depth balancing:** The difference in leaf depth (distance from root to leaf) does not exceed 1 layer. This ensures a balanced tree and efficient memory utilisation.
   - **Filling layers from left to right:** The last layer of the tree is filled from left to right, without skips. This ensures efficient storage of data as an array.

4. **Insertion and extraction operations:**
   - **Insert (insert):** The operation of adding a new element to a data structure. In the context of a priority queue, an element is inserted based on its priority.
   - **extract:** The operation of removing and returning the highest priority element from the data structure.

5. **Methods to maintain the data structure:**
   - **Method `up(i)`:** Moves an element up the tree, ensuring that the data structure is correct after insertion.
   - ** **Method `down(i)`:** Drops an element down the tree, ensuring that the data structure is correct after extraction.

6. **Exception `std::out_of_range`:**
   An exception used in C++ to signal when an index is out of range. In this context, used to handle retrieval attempts from an empty priority queue.

7. **Testing using Google Test:**
   Google Test is a framework for writing tests in the C++ programming language. Testing using it provides test automation and ensures code reliability.

## <p align=‘centre’>Description of the algorithms used:</p>
1. **Binary heap (Heap):**
In my library I implemented a binary heap via `std::vector`. Where the n-th element corresponded to the elements 2n+1 and 2n+2.

<p align=‘centre’>
  <img src=‘https://media.geeksforgeeks.org/wp-content/cdn-uploads/binaryheap.png’ alt=‘Binary heap via vector’>
</p>

2. **Insert method (insert):**
Responsible for inserting a new element into the priority queue. This method adds the element to the end of the vector and then calls the up method, which brings the element up the tree to enforce the binary heap conditions.
    - **Auxiliary up method:**
    Responsible for lifting the element up the tree to restore the binary heap conditions after insertion. This method executes a loop, comparing the value of the current element with its parent and swapping them as necessary until the element reaches the correct position.

``cpp.
void priorityQueue::insert(int value) {
	a.push_back(value);
	up(a.size() - 1);
}

void priorityQueue::up(int i) {
	while (a[i] > a[(i - 1) / 2] && i != 0) {
		swap(a[(i - 1) / 2], a[i]);
		i = (i - 1) / 2;
	}
}
```

3. **Extract method:**
Responsible for extracting the highest priority element from the priority queue. This method extracts the value from the root, replaces the root with the last element of the vector, and then calls the down method which drops the new root down the tree to restore the binary heap conditions. The case of attempting to retrieve from an empty queue is also taken into account, the `std::out_of_range` exception is created for this case 
    - **Auxiliary method down:**
    Responsible for dropping an element down the tree to restore the binary heap conditions after an extraction. This method executes a loop, comparing the value of the current element with its descendants and swapping its position with the largest of the descendants, if necessary, until the element reaches the correct position.

``cpp.
int priorityQueue::extract() {
	if (isEmpty()) {
		throw out_of_range(‘Priority queue is empty’);
	}

	int value = a[0];
	a[0] = a[a.size() - 1];
	a.pop_back();
	down(0);
	return value;
}

void priorityQueue::down(int i) {
	while (2 * i + 1 < a.size()) {
		int maxChild = 2 * i + 1;
		if (maxChild + 1 < a.size() && a[maxChild] < a[maxChild + 1])
			maxChild++;
		if (a[i] >= a[maxChild])
			break;
		swap(a[maxChild],a[i]);
		i = maxChild;
	}
}
```
   
4. **Method for checking if the queue is empty (isEmpty):**
Checks if the priority queue is empty. Returns true if vector a is empty and false otherwise.

``cpp
bool priorityQueue::isEmpty() const {
	return a.empty();
}
```

5. **Algorithm for priority queue operation:**

    1. **Initialisation:**
       - Create an instance of the `priorityQueue` class.
       - The inner vector `a` is used to store items as a binary heap.
    
    2. **Element Insertion:**
       - A new element is added to the end of the vector.
       - The `up` method is called to restore the conditions of the binary heap.
    
    3. **Element Retrieval:**
       - Checks if there are elements in the heap.
       - Extracting the value from the root (with the highest priority).
       - Replacing the root with the last element of the vector.
       - Calling the `down` method to restore the conditions of the binary heap.
    
    4. **Lifting an element up:**.
       - Until the current element is greater than the parent and the root is reached:
         - Swap the current element and its parent.
         - Update the index of the current element.
    
    5. **Lowering an element down:**
       - As long as there is at least one descendant of the current element:
         - We select the maximum of the descendants.
         - If the current element is less than the selected descendant, swap them.
         - Update the index of the current element.
    
    6. **Checking the presence of elements:**
       - Check if the heap is empty based on the emptiness of the vector `a`.

## <p align=‘centre’>Test results:</p>
The testing was done using the Google test framework. The test system consists of 5 tests: 2 basic tests for insertion and extraction, test for insertion and extraction with identical elements, test for insertion and extraction with negative elements, test for extraction from an empty queue. All tests passed successfully.

<p align=‘centre’>
  <img src=‘https://github.com/IRomanchuk06/PriorityQueue/blob/main/TestPriorityQueue.png?raw=true’ alt=‘Library tests’>
</p>

**Test System:**

```cpp
#include ‘pch.h’
#include <gtest/gtest.h>
#include ‘../priorituQueue/priorityQueue.h’
using namespace std;

//basic insertion and extraction test
TEST(PriorityQueueTest, InsertAndExtract1) {
    priorityQueue pq;

    EXPECT_TRUE(pq.isEmpty());

    pq.insert(10);
    pq.insert(5);
    pq.insert(20);

    EXPECT_FALSE(pq.isEmpty());

    EXPECT_EQ(pq.extract(), 20);
    EXPECT_EQ(pq.extract(), 10);
    EXPECT_EQ(pq.extract(), 5);

    EXPECT_TRUE(pq.isEmpty());
}

//basic insertion and extraction test
TEST(PriorityQueueueTest, InsertAndExtract2) {
    priorityQueue pq;

    pq.insert(10);
    pq.insert(5);
    pq.insert(20);
    pq.insert(15);
    pq.insert(25);

    EXPECT_EQ(pq.extract(), 25);
    EXPECT_EQ(pq.extract(), 20);
    EXPECT_EQ(pq.extract(), 15);
    EXPECT_EQ(pq.extract(), 10);
    EXPECT_EQ(pq.extract(), 5);

    EXPECT_TRUE(pq.isEmpty());
}
//test for insertion and extraction with identical elements
TEST(PriorityQueueueTest, InsertAndExtractWithDuplicates) {
    priorityQueue pq;
    pq.insert(10);
    pq.insert(5);
    pq.insert(10);
    pq.insert(8);

    EXPECT_EQ(pq.extract(), 10);
    EXPECT_EQ(pq.extract(), 10);
    EXPECT_EQ(pq.extract(), 8);
    EXPECT_EQ(pq.extract(), 5);
    EXPECT_TRUE(pq.isEmpty());
}

//test for extracting from an empty queue
TEST(PriorityQueueueTest, ExtractFromEmptyQueueue) {
    priorityQueue pq;

    EXPECT_THROW(pq.extract(), std::out_of_range);
}

//test for insertion and extraction with negative elements
TEST(PriorityQueueueTest, InsertWithNegativeNumbers) {
    priorityQueue pq;
    
    pq.insert(10);
    pq.insert(-5);
    pq.insert(25);
    pq.insert(-10);
    pq.insert(-2);

    EXPECT_EQ(pq.extract(), 25);
    EXPECT_EQ(pq.extract(), 10);
    EXPECT_EQ(pq.extract(), -2);
    EXPECT_EQ(pq.extract(), -5);
    EXPECT_EQ(pq.extract(), -10);
    EXPECT_TRUE(pq.isEmpty());
}

int main(int argc, char** argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```
## <p align=‘centre’>Conclusion:</p>
**Learned:**
In the course of the lab work I deepened my knowledge in the principles of max-heap priority queues. By understanding the technique of ‘lifting’ and ‘dropping’ elements, I learnt how to effectively manage a data structure where the max-heap element is located at the root. I also learnt the use of standard C++ containers, in particular the use of `std::vector` to represent a binary heap.

**Done:**
My result was a successful implementation of a library for the priority queue. I implemented the insert, extract, and empty check operations intelligently, ensuring that the data structure worked efficiently and correctly. Writing tests using Google Test allowed me to make sure that my implementation was reliable and correct.

**Mastered:**
This lab work gave me a deep understanding of priority queue design and implementation. Experience with testing frameworks, in this case Google Test, gave me the opportunity to test the functionality of my code, which was an important step in ensuring its correctness. Using standard exceptions for error handling became an integral part of my experience in creating reliable software solutions.

As a result, the lab work not only expanded my theoretical knowledge, but also gave me practical experience in the field of data structures development, which has a positive effect on my programming skills.

## <p align=‘centre’>Sources used:</p>
1. https://neerc.ifmo.ru/wiki/index.php?title=Приоритетные_очереди#.D0.A0.D0.B5.D0.B0.D0.BB.D0.B8.D0.B7.D0.B0.D1.86.D0.B8.D0.B8 (definitions of concepts).
2. https://www.youtube.com/watch?v=5mD-rhaYF4U&t=209s (library creation).
3. https://www.youtube.com/watch?v=o1ZDXf7NGN4&t=868s (implementing algorithms in C++).
4. https://github.com/google/googletest (about Google test: installation, use).
5. https://chat.openai.com (help in mastering the material)

