# cpp库

## chrono

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
#include <iostream>
#include <chrono>
#include <thread>
#include <vector>
#include <algorithm>
using namespace std;

void basicChronoDemo() {
    cout << "=== chrono 基础概念演示 ===" << endl;
    
    // 1. 获取当前时间点
    auto now = chrono::high_resolution_clock::now();
    cout << "获取当前时间点完成" << endl;
    
    // 2. 模拟一些耗时操作
    cout << "执行耗时操作..." << endl;
    this_thread::sleep_for(chrono::milliseconds(100));  // 休眠100毫秒
    
    // 3. 获取结束时间点
    auto end = chrono::high_resolution_clock::now();
    
    // 4. 计算时间差
    auto duration = end - now;
    
    // 5. 转换为不同单位
    auto nanoseconds = chrono::duration_cast<chrono::nanoseconds>(duration);
    auto microseconds = chrono::duration_cast<chrono::microseconds>(duration);
    auto milliseconds = chrono::duration_cast<chrono::milliseconds>(duration);
    auto seconds = chrono::duration_cast<chrono::seconds>(duration);
    
    cout << "耗时统计:" << endl;
    cout << "  纳秒: " << nanoseconds.count() << " ns" << endl;
    cout << "  微秒: " << microseconds.count() << " μs" << endl;
    cout << "  毫秒: " << milliseconds.count() << " ms" << endl;
    cout << "  秒数: " << seconds.count() << " s" << endl;
}

void clockTypesDemo() {
    cout << "\n=== 不同时钟类型演示 ===" << endl;
    
    // 1. system_clock - 系统时钟（可以转换为日期时间）
    auto system_now = chrono::system_clock::now();
    time_t system_time = chrono::system_clock::to_time_t(system_now);
    cout << "系统时间: " << ctime(&system_time);
    
    // 2. steady_clock - 单调时钟（不会倒退，适合测量时间间隔）
    auto steady_start = chrono::steady_clock::now();
    this_thread::sleep_for(chrono::milliseconds(50));
    auto steady_end = chrono::steady_clock::now();
    auto steady_duration = chrono::duration_cast<chrono::milliseconds>(steady_end - steady_start);
    cout << "steady_clock 测量: " << steady_duration.count() << " ms" << endl;
    
    // 3. high_resolution_clock - 高精度时钟（通常是steady_clock的别名）
    auto hr_start = chrono::high_resolution_clock::now();
    // 执行一些快速操作
    volatile int sum = 0;
    for (int i = 0; i < 100000; ++i) {
        sum += i;
    }
    auto hr_end = chrono::high_resolution_clock::now();
    auto hr_duration = chrono::duration_cast<chrono::nanoseconds>(hr_end - hr_start);
    cout << "high_resolution_clock 测量: " << hr_duration.count() << " ns" << endl;
}

void durationDemo() {
    cout << "\n=== duration 时间段演示 ===" << endl;
    
    // 直接创建不同单位的时间段
    chrono::nanoseconds ns(1000);
    chrono::microseconds us(500);
    chrono::milliseconds ms(200);
    chrono::seconds sec(3);
    chrono::minutes min(2);
    chrono::hours hr(1);
    
    cout << "预定义时间段:" << endl;
    cout << "  1000 nanoseconds = " << ns.count() << " ns" << endl;
    cout << "  500 microseconds = " << us.count() << " μs" << endl;
    cout << "  200 milliseconds = " << ms.count() << " ms" << endl;
    cout << "  3 seconds = " << sec.count() << " s" << endl;
    cout << "  2 minutes = " << min.count() << " min" << endl;
    cout << "  1 hour = " << hr.count() << " h" << endl;
    
    // 时间段转换
    cout << "\n时间段转换:" << endl;
    cout << "  3秒 = " << chrono::duration_cast<chrono::milliseconds>(sec).count() << " 毫秒" << endl;
    cout << "  2分钟 = " << chrono::duration_cast<chrono::seconds>(min).count() << " 秒" << endl;
    cout << "  1小时 = " << chrono::duration_cast<chrono::minutes>(hr).count() << " 分钟" << endl;
    
    // 时间段运算
    auto total = sec + chrono::milliseconds(500);
    cout << "  3秒 + 500毫秒 = " << chrono::duration_cast<chrono::milliseconds>(total).count() << " 毫秒" << endl;
}

// 性能测试工具类
class PerformanceTimer {
private:
    chrono::high_resolution_clock::time_point start_time;
    string timer_name;
    
public:
    PerformanceTimer(const string& name) : timer_name(name) {
        start_time = chrono::high_resolution_clock::now();
    }
    
    ~PerformanceTimer() {
        auto end_time = chrono::high_resolution_clock::now();
        auto duration = chrono::duration_cast<chrono::microseconds>(end_time - start_time);
        cout << timer_name << " 耗时: " << duration.count() << " 微秒" << endl;
    }
    
    // 手动停止并返回耗时
    long long stop() {
        auto end_time = chrono::high_resolution_clock::now();
        auto duration = chrono::duration_cast<chrono::microseconds>(end_time - start_time);
        return duration.count();
    }
};

void performanceTestDemo() {
    cout << "\n=== 性能测试演示 ===" << endl;
    
    // 方法1：RAII方式（推荐）
    {
        PerformanceTimer timer("向量排序");
        vector<int> data;
        for (int i = 1000; i > 0; i--) {
            data.push_back(i);
        }
        sort(data.begin(), data.end());
    } // timer析构时自动输出结果
    
    // 方法2：手动计时
    auto start = chrono::high_resolution_clock::now();
    
    // 测试向量插入性能
    vector<int> vec;
    for (int i = 0; i < 10000; i++) {
        vec.push_back(i);
    }
    
    auto end = chrono::high_resolution_clock::now();
    auto duration = chrono::duration_cast<chrono::microseconds>(end - start);
    cout << "向量插入 耗时: " << duration.count() << " 微秒" << endl;
    
    // 方法3：使用lambda包装
    auto timeFunction = [](const string& name, function<void()> func) {
        auto start = chrono::high_resolution_clock::now();
        func();
        auto end = chrono::high_resolution_clock::now();
        auto duration = chrono::duration_cast<chrono::nanoseconds>(end - start);
        cout << name << " 耗时: " << duration.count() << " 纳秒" << endl;
    };
    
    timeFunction("数学计算", []() {
        volatile double result = 0;
        for (int i = 0; i < 1000; i++) {
            result += sqrt(i) * sin(i);
        }
    });
}

void practicalExamples() {
    cout << "\n=== 实际应用示例 ===" << endl;
    
    // 示例1：函数执行时间统计
    auto measureFunction = [](const string& name, function<void()> func) -> long long {
        auto start = chrono::high_resolution_clock::now();
        func();
        auto end = chrono::high_resolution_clock::now();
        auto duration = chrono::duration_cast<chrono::microseconds>(end - start);
        cout << name << ": " << duration.count() << " μs" << endl;
        return duration.count();
    };
    
    // 比较不同算法的性能
    vector<int> data(1000);
    iota(data.begin(), data.end(), 1);
    random_shuffle(data.begin(), data.end());
    
    vector<int> data1 = data, data2 = data;
    
    long long time1 = measureFunction("冒泡排序", [&]() {
        // 简化的冒泡排序
        for (size_t i = 0; i < data1.size(); i++) {
            for (size_t j = 0; j < data1.size() - 1; j++) {
                if (data1[j] > data1[j + 1]) {
                    swap(data1[j], data1[j + 1]);
                }
            }
        }
    });
    
    long long time2 = measureFunction("标准库排序", [&]() {
        sort(data2.begin(), data2.end());
    });
    
    cout << "性能差异: " << (double)time1 / time2 << " 倍" << endl;
    
    // 示例2：超时控制
    cout << "\n超时控制示例:" << endl;
    auto timeout_start = chrono::steady_clock::now();
    const auto timeout_duration = chrono::milliseconds(100);
    
    int count = 0;
    while (true) {
        auto now = chrono::steady_clock::now();
        if (now - timeout_start > timeout_duration) {
            cout << "超时退出，执行了 " << count << " 次循环" << endl;
            break;
        }
        count++;
        // 模拟一些工作
        this_thread::sleep_for(chrono::microseconds(10));
    }
}

void chronoSyntaxGuide() {
    cout << "\n=== chrono 语法速查 ===" << endl;
    
    cout << "1. 基本用法模板:" << endl;
    cout << "   auto start = chrono::high_resolution_clock::now();" << endl;
    cout << "   // ... 执行代码 ..." << endl;
    cout << "   auto end = chrono::high_resolution_clock::now();" << endl;
    cout << "   auto duration = chrono::duration_cast<单位>(end - start);" << endl;
    cout << "   cout << duration.count() << endl;" << endl;
    
    cout << "\n2. 常用时间单位:" << endl;
    cout << "   chrono::nanoseconds   - 纳秒" << endl;
    cout << "   chrono::microseconds  - 微秒" << endl;
    cout << "   chrono::milliseconds  - 毫秒" << endl;
    cout << "   chrono::seconds       - 秒" << endl;
    cout << "   chrono::minutes       - 分钟" << endl;
    cout << "   chrono::hours         - 小时" << endl;
    
    cout << "\n3. 常用时钟类型:" << endl;
    cout << "   high_resolution_clock - 最高精度（推荐用于性能测试）" << endl;
    cout << "   steady_clock         - 单调时钟（不会倒退）" << endl;
    cout << "   system_clock         - 系统时钟（可转换为日期时间）" << endl;
    
    cout << "\n4. 实用技巧:" << endl;
    cout << "   - 使用 auto 简化类型声明" << endl;
    cout << "   - 用 duration_cast 进行单位转换" << endl;
    cout << "   - 用 count() 获取数值结果" << endl;
    cout << "   - 建议用 steady_clock 测量时间间隔" << endl;
}

int main() {
    basicChronoDemo();
    clockTypesDemo();
    durationDemo();
    performanceTestDemo();
    practicalExamples();
    chronoSyntaxGuide();
    
    return 0;
}
```

