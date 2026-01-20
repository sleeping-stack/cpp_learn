# vector类

## 定义与初始化

`std::vector<int> vec;  // 定义一个空的整数向量`  

`std::vector<std::string> words;    // 定义一个空的字符串向量`

~~~C++
#include <iostream>
#include <vector>

int main() {
    // 默认初始化
    std::vector<int> vec1;

    // 指定大小和默认值
    std::vector<int> vec2(5, 10);

    // 使用初始化列表
    std::vector<int> vec3 = {1, 2, 3, 4, 5};

    // 拷贝构造
    std::vector<int> vec4(vec3);

    // 移动构造，不能再访问vec4
    std::vector<int> vec5(std::move(vec4));

    // 输出vec2
    std::cout << "vec2: ";
    for(auto num : vec2) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 输出vec3
    std::cout << "vec3: ";
    for(auto num : vec3) {  // 依次将vec3中的元素赋给num
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 输出vec5
    std::cout << "vec5: ";
    for(auto num : vec5) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
~~~

## 向量的大小和容量

~~~C++
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3};

    std::cout << "Size: " << vec.size() << std::endl;       // 输出: 3
    
    // capacity() 并不一定精确匹配 size()，它表示在需要重新分配内存之前，向量可以容纳的元素数量
    std::cout << "Capacity: " << vec.capacity() << std::endl; // 输出: 3（或更大，取决于实现）

    std::cout << "Is empty? " << (vec.empty() ? "Yes" : "No") << std::endl; // 输出: No

    vec.reserve(10); // 预留容量
    std::cout << "After reserve(10), Capacity: " << vec.capacity() << std::endl; // 输出: 10

    vec.shrink_to_fit(); // 收缩到适合大小
    std::cout << "After shrink_to_fit(), Capacity: " << vec.capacity() << std::endl; // 输出: 3

    return 0;
}
~~~

- 立即回收容量的机制：这种方式能立即回收内存，特别适用于那些不再需要大容量的vector对象，可以避免内存占用过高的问题

~~~C++
    // 通过局部作用域清空vec的capacity
    {
        std::vector<int> empty_vec;
        empty_vec.swap(vec); 
    }
~~~

## 基本操作

### 添加和删除元素

- push_back()：在向量末尾添加一个元素。

- pop_back()：移除向量末尾的元素。

- insert()：在指定位置插入元素。

- erase()：移除指定位置的元素或范围内的元素。

- clear()：移除所有元素。

~~~C++
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec;

    // 使用push_back在向量末尾添加一个元素
    vec.push_back(10);
    vec.push_back(20);
    vec.push_back(30);

    std::cout << "After push_back: ";
    for(auto num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl; // 输出: 10 20 30 

    // 使用pop_back移除最后一个元素
    vec.pop_back();

    std::cout << "After pop_back: ";
    for(auto num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl; // 输出: 10 20 

    // 在第二个位置前插入25
    vec.insert(vec.begin() + 1, 25);

    std::cout << "After insert: ";
    for(auto num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl; // 输出: 10 25 20 

    // 删除第二个元素（25）
    vec.erase(vec.begin() + 1);

    std::cout << "After erase: ";
    for(auto num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl; // 输出: 10 20 

    // 清空向量
    vec.clear();
    std::cout << "After clear, size: " << vec.size() << std::endl; // 输出: 0

    return 0;
}
~~~

### 访问元素

- operator[]：通过索引访问元素。

- at()：通过索引访问元素，带边界检查。

- front()：访问第一个元素。

- back()：访问最后一个元素。

~~~C++
#include <iostream>
#include <vector>

int main() {
    std::vector<std::string> fruits = {"Apple", "Banana", "Cherry"};

    // 使用operator[]访问元素
    std::cout << "First fruit: " << fruits[0] << std::endl; // 输出: Apple

    // 使用at()访问元素，带边界检查
    try {
        std::cout << "Second fruit: " << fruits.at(1) << std::endl; // 输出: Banana
        std::cout << "Invalid fruit: " << fruits.at(5) << std::endl; // 抛出异常
    }
    catch(const std::out_of_range& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    // 使用front()和back()
    std::cout << "Front: " << fruits.front() << std::endl; // 输出: Apple
    std::cout << "Back: " << fruits.back() << std::endl;   // 输出: Cherry

    return 0;
}
~~~

### 遍历向量

~~~C++
    // 使用迭代器
    std::cout << "Using iterators: ";
    for(auto it = numbers.begin(); it != numbers.end(); ++it) {
        std::cout << *it << " ";
    }
~~~

### 修改元素

~~~C++
#include <iostream>
#include <vector>

int main() {
std::vector<int> vec = {10, 20, 30, 40, 50};

    // 通过索引修改元素
    vec[2] = 35;

    // 使用 at() 修改元素
    vec.at(4) = 55;

    // 使用迭代器修改元素
    for(auto it = vec.begin(); it != vec.end(); ++it) {
        if(*it == 20) {
            *it = 25;
        }
    }
}
~~~

## 高级用法

### 嵌套向量(二维向量)

~~~C++
int main() {
    // 定义一个3x4的二维向量，初始化为0
    std::vector<std::vector<int>> matrix(3, std::vector<int>(4, 0));

    // 填充矩阵
    for(int i = 0; i < 3; ++i) {
        for(int j = 0; j < 4; ++j) {
            matrix[i][j] = i * 4 + j + 1;
        }
    }

    // 输出矩阵
    std::cout << "Matrix:" << std::endl;
    for(auto row : matrix) {
        for(auto elem : row) {
            std::cout << elem << "\t";
        }
        std::cout << std::endl;
    }

    return 0;
}
~~~

### 与其他数据结构结合

- 如与结构体

~~~C++
// 定义学生结构体
struct Student {
    int id;
    std::string name;
    float grade;
};

int main() {
    // 定义一个学生向量
    std::vector<Student> students;

    // 添加学生
    students.push_back({1001, "Alice", 89.5});
    students.push_back({1002, "Bob", 92.0});
    students.push_back({1003, "Charlie", 85.0});

    // 遍历并输出学生信息
    for(const auto& student : students) {
        std::cout << "ID: " << student.id 
                  << ", Name: " << student.name 
                  << ", Grade: " << student.grade << std::endl;
    }

    return 0;
}
~~~

## 常用算法与向量

### 排序

~~~C++
#include <algorithm>
    
// 使用sort()排序
std::sort(numbers.begin(), numbers.end());

// 使用sort()并传入lambda表达式进行降序排序
std::sort(numbers.begin(), numbers.end(), [](int a, int b) {
    return a > b;
});

std::soft(numbers.begin(), numbers.end(), std::greater<int>());
~~~

### 反转

~~~C++
#include <algorithm>

// 使用reverse()反转向量
std::reverse(numbers.begin(), numbers.end());
~~~

### 查找

~~~C++
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<std::string> fruits = {"Apple", "Banana", "Cherry", "Date"};
    std::string target = "Cherry";

    // 使用find()查找元素
    auto it = std::find(fruits.begin(), fruits.end(), target);

    if(it != fruits.end()) {
        std::cout << target << " found at position " << std::distance(fruits.begin(), it) << std::endl;
    }
    else {
        std::cout << target << " not found." << std::endl;
    }

    return 0;
}
~~~

## 向量的性能与优化

- 频繁的内存分配可能会影响性能

### 使用reserve()预分配内存可以减少内存分配的次数

`vec.reserve(100);  // 预分配空间，避免多次内存分配`

### 收缩容量

` vec.shrink_to_fit();  // 收缩到实际大小，释放多余的内存`

