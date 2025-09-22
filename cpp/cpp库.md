# cpp库

## chrono（高精度时间测量与处理）

`chrono` 是 C++11 引入的时间库，用于高精度的时间测量和处理。

### 功能

chrono提供了类型安全的时间测量功能，主要用于：

- 性能测试：测量代码执行时间
- 时间间隔计算：精确计算时间差
- 超时控制：实现超时机制
- 时间戳：获取高精度时间点

### 核心组件

**时钟类型（Clocks）**：

- `high_resolution_clock` - 最高精度时钟（性能测试首选）
- `steady_clock` - 单调时钟（不会倒退）
- `system_clock` - 系统时钟（可转换为日期时间）

**时间点（Time Points）**：

- 表示特定的时间瞬间
- 通过 `clock::now()` 获取

**时间段（Duration）**：

- 表示两个时间点之间的间隔
- 支持纳秒到小时的各种单位

### 常用语法模板

```c++
#include <chrono>

// 1. 基本计时模板
auto start = chrono::high_resolution_clock::now();
// 执行要测试的代码
auto end = chrono::high_resolution_clock::now();
auto duration = chrono::duration_cast<chrono::microseconds>(end - start);
cout << "耗时: " << duration.count() << " 微秒" << endl;

// 2. 创建时间间隔
this_thread::sleep_for(chrono::milliseconds(100));  // 休眠100毫秒
```

```c++
#include <bits/stdc++.h>
#include <chrono>

using namespace std;

int main() {
    cout << "=== chrono 基础用法演示 ===" << endl;
    
    // 1. 最基本的计时用法
    cout << "\n1. 基本计时:" << endl;
    auto start = chrono::high_resolution_clock::now();
    
    // 模拟一些计算
    int sum = 0;
    for (int i = 0; i < 1000000; i++) {
        sum += i;
    }
    
    auto end = chrono::high_resolution_clock::now();
    auto duration = chrono::duration_cast<chrono::microseconds>(end - start);
    
    cout << "计算耗时: " << duration.count() << " 微秒" << endl;
    cout << "结果: " << sum << endl;
    
    // 2. 测试排序性能
    cout << "\n2. 排序性能测试:" << endl;
    
    // 创建测试数据
    vector<int> data;
    for (int i = 1000; i > 0; i--) {
        data.push_back(i);
    }
    
    cout << "排序 1000 个数字..." << endl;
    
    start = chrono::high_resolution_clock::now();
    sort(data.begin(), data.end());
    end = chrono::high_resolution_clock::now();
    
    auto sort_time = chrono::duration_cast<chrono::microseconds>(end - start);
    cout << "排序耗时: " << sort_time.count() << " 微秒" << endl;
    
    // 3. 不同时间单位的转换
    cout << "\n3. 时间单位转换:" << endl;
    auto total_duration = duration + sort_time;
    
    auto nanoseconds = chrono::duration_cast<chrono::nanoseconds>(total_duration);
    auto microseconds = chrono::duration_cast<chrono::microseconds>(total_duration);
    auto milliseconds = chrono::duration_cast<chrono::milliseconds>(total_duration);
    
    cout << "总耗时:" << endl;
    cout << "  " << nanoseconds.count() << " 纳秒" << endl;
    cout << "  " << microseconds.count() << " 微秒" << endl;
    cout << "  " << milliseconds.count() << " 毫秒" << endl;
    
    // 4. 常用的计时模板
    cout << "\n4. 实用计时模板:" << endl;
    cout << "auto start = chrono::high_resolution_clock::now();" << endl;
    cout << "// 你的代码" << endl;
    cout << "auto end = chrono::high_resolution_clock::now();" << endl;
    cout << "auto time = chrono::duration_cast<chrono::microseconds>(end - start);" << endl;
    cout << "cout << time.count() << \" 微秒\" << endl;" << endl;
    
    return 0;
}
```

