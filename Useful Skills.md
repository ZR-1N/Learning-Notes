# Useful Skills

## 负数取模

在C++中，负数取模的结果**可能是负数**（这取决于编译器实现），比如：

```cpp
//-5 % 3 可能等于 -2，而不是我们期望的 1
```



这种bug在竞赛中非常常见，特别是在组合数学、容斥原理等需要**大量加减运算**的题目中。

```cpp
ans = ((ans % modnum) + modnum) % modnum;
```

以上代码是处理负数取模的常用做法，这是标准且简洁的解决方案。在算法竞赛中，很多人习惯**在所有可能产生负数的地方都加上这行代码**，**确保结果始终为正**。

除了以上修正方式外，还有几种预防性的写法：

- 逐步取模

  ```cpp
  ans = a;
  ans = (ans + b) % modnum;
  ans = (ans - c + modnum) % modnum;  // 减法时立即处理负数
  ans = (ans - d + modnum) % modnum;
  ```

- 保证每次减法都是正数

  ```cpp
  ans = ((a + b) % modnum - (c + d) % modnum + modnum) % modnum;
  ```

- 使用辅助函数

```cpp
long long add(long long x, long long y) {
    return (x + y) % modnum;
}

long long sub(long long x, long long y) {
    return ((x - y) % modnum + modnum) % modnum;
}

// 使用时：
ans = sub(add(a, b), add(c, d));
```

