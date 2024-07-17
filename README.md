# Custom Vector Implementation in C++
## Description
This project provides a custom implementation of a vector container in C++, akin to 'std::vector'. This vector class manages dynamic arrays, supporting efficient random access, insertion, and deletion operations. It includes a custom memory management class 'RawMemory' to handle raw memory allocation and deallocation, ensuring exception safety and optimal performance.

# Instructions for Deployment/Usage
To use this vector implementation in your C++ project, follow these steps:

1. Clone the Repository or Download the Header Files:
Download the header files vector.h and raw_memory.h (if separated) from this repository.

2. Include the Header Files in Your Project:
```cpp
#include "vector.h"
```
3. Compile Your Project:
Ensure your compiler supports C++17 and above. Compile your project using your preferred build system or compiler. ```g++ -std=c++17 -o your_program your_program.cpp```
4. Run Your Program: ```./your_program```

# System Requirements
- C++ Version: C++17 or above.
- Dependencies: Standard Template Library (STL) headers such as '<cassert>', '<cstdlib>', '<new>', '<utility>', '<memory>', and '<algorithm>'.

# Example Test Case Breakdown
Here's an example test case demonstrating basic usage and exception safety for the vector:
```cpp
#include "vector.h"
#include <cassert>

// Test case for basic vector operations
void Test() {
    Vector<int> vec(5);
    vec[0] = 10;
    vec[1] = 20;
    vec[2] = 30;
    vec[3] = 40;
    vec[4] = 50;
    assert(vec.Size() == 5);
    assert(vec[0] == 10);
    assert(vec[4] == 50);

    vec.PushBack(60);
    assert(vec.Size() == 6);
    assert(vec[5] == 60);

    vec.PopBack();
    assert(vec.Size() == 5);

    vec.Insert(vec.begin() + 2, 25);
    assert(vec.Size() == 6);
    assert(vec[2] == 25);

    vec.Erase(vec.begin() + 2);
    assert(vec.Size() == 5);
    assert(vec[2] == 30);
}

// Test case for exception safety with custom type
struct ThrowOnCopy {
    ThrowOnCopy() = default;
    explicit ThrowOnCopy(int& copy_counter) noexcept : countdown_ptr(&copy_counter) {}
    ThrowOnCopy(const ThrowOnCopy& other) : countdown_ptr(other.countdown_ptr) {
        if (countdown_ptr) {
            if (*countdown_ptr == 0) {
                throw std::bad_alloc();
            } else {
                --(*countdown_ptr);
            }
        }
    }
    ThrowOnCopy& operator=(const ThrowOnCopy& rhs) = delete;
    int* countdown_ptr = nullptr;
};

void TestExceptionSafety() {
    bool exception_was_thrown = false;
    for (int max_copy_counter = 10; max_copy_counter >= 0; --max_copy_counter) {
        Vector<ThrowOnCopy> vec(3);
        try {
            int copy_counter = max_copy_counter;
            vec.Insert(vec.cbegin(), ThrowOnCopy(copy_counter));
            assert(vec.Size() == 4);
        } catch (const std::bad_alloc&) {
            exception_was_thrown = true;
            assert(vec.Size() == 3);
            break;
        }
    }
    assert(exception_was_thrown);
}

int main() {
    Test();
    TestExceptionSafety();
    return 0;
}
```
# System Requirements
- Compiler: GCC 7.0 or later, Clang 5.0 or later, MSVC 2017 or later.
- Dependencies: C++ Standard Library
# Future Plans
- Implement additional member functions for Vector, such as shrink_to_fit and clear.
- Add support for custom allocators.
- Optimize current methods for better performance with large data sets.
- Provide a more comprehensive test suite.
# Technology Stack
- Language: C++
- C++ Standard: C++17
- Libraries: Standard Template Library (STL)

