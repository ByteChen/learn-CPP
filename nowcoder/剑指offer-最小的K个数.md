## [最小的K个数](https://www.nowcoder.com/questionTerminal/6a296eb82cf844ca8539b57c23e6e9bf)
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

## 代码
#### 方法1：利用快速排序中的partition()，时间复杂度 O(n)
```C++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int n = input.size();
        if(n < k)
            return {};
        if(n == k)
            return input;
        int index = partition(input, 0, n-1);
        int low = 0, high = n-1;
        while(index != k-1)
        {
            if(index < k-1) {
                low = index + 1;
                index = partition(input, low, high);
            }
            else {
                high = index - 1;
                index = partition(input, low, high);
            }
        }
        vector<int> res(k);
        copy(input.begin(), input.begin()+k, res.begin());
        return res;
    }
    
    int partition(vector<int>& input, int low, int high) {
        if(low > high) return -1;
        int index = rand() % (high - low) + low;
        int key = input[index];
        std::swap(input[index], input[high]);
        int small = low - 1;
        while(low < high) {
            if(input[low] < key) {
                ++ small;
                std::swap(input[small], input[low]);
            }
            low ++;
        }
        ++small;
        std::swap(input[small], input[high]);
        return small;
    }
};
```

#### 方法二：利用大顶堆
* 大顶堆可以用STL中的**set** , **multiset** 实现，他们都是使用红黑树，但容器中元素个数为k个时，插入、删除复杂度为 O(k) ，所以总的复杂度是 O(nlogk). 也可以用**priority_queue**实现，不过要注意，如下：
```C++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int n = input.size();
        if(n < k || k <= 0)
            return {};
        if(n == k)
            return input;
        priority_queue<int, vector<int>, less<int> > q;     //注意！大顶堆用less，小顶堆用greater！！！
        int i;
        for(i = 0; i<k; i++)
            q.push(input[i]);
        for(; i<n; i++) {
            if(input[i] < q.top()) {
                q.pop();
                q.push(input[i]);
            }
        }
        vector<int> res;
        //copy(q.begin(), q.end(), res.begin());    //由于priority_queue没有迭代器，所以这些注释掉的方法都不能用
        /*for (auto each : q)
            res.push_back(each);*/
        while(!q.empty()) {
            res.push_back(q.top());
            q.pop();
        }
        return res;
    }
};
```

## 总结
* 参考[使用priority_queue建立小顶推](http://blog.csdn.net/trochiluses/article/details/39589879)
* 参考[优先队列详解priority_queue](https://www.cnblogs.com/Mimick/p/6028654.html)， 这篇文章还包含一些不错的acm题目
* 关于随机数的产生：rand（）根据时间种子生成随机数，当不改变随机种子时，实际上每次运行都是按照特定的顺序生成相同的伪随机数序列。因为应该每次调用rand()前，使用srand()更新随机种子。而为了让srand()每次产生的随机种子不一样，则它的参数应该不一样。使用time()获得当前时间，因为当前时间总是不一样的，所以srand()每次都能产生不一样的随机种子。
```C++
#include<cstdlib>   //rand(), srand()函数
#include<ctime>     //time()函数，获得当前时间
    ·
    ·
srand(time(0));     //或者 srand(time(NULL))。原型：time_t time (time_t* timer); time（）的参数是一个time_t* 指针，若不为0或者NULL，就会把时间值写到指针所指的内存。
int a = rand()；     //从而得到一个随机数
    ·
    ·
```

