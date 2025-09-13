# Transistor

晶体管
可以处于两种状态之一，打开或关闭
当晶体管导通或打开时，电流可以通过，晶体管可以组成逻辑门（Logic Gates）

# Logic Gates

<figure>
    <img src="https://www.autodesk.com/products/fusion-360/blog/wp-content/uploads/2023/07/AdobeStock_613706823-768x461.jpeg" alt="Logic Gates" width=50%>
    <figcaption>Logic Gates</figcaption>
</figure>



## NOT Gate

输出取输入的反

## AND Gate

只有所有输入都是1时，输出才是1
A⋅B

## OR Gate

只要有一个输入是1，输出就是1
A+B

## NAND Gate（与非门）

输出是与门的反
只有所有输入都是1时，输出才是0，否则输出1
**NADN是功能完备的，可以用NAND门组合出任意逻辑门（NOT，ADN，OR，XOR）**

## XOR Gate（异或门/Exclusive Or）

输入不同时输出1，输入相同时输出0
**不同为真，相当于“模2加法”**
A⊕B

# Arithmetics

在掌握了逻辑门以后，就可以组合它们实现算术功能
## Half Adder（半加器）

半加器用来实现**两个一位二进制数的加法**，有两个输出

- Sum:用XOR来实现
- Carry:用AND实现

半加器不能处理“来自低位的进位输入”，只适合加两个独立位

## Full Adder（全加器）

全加器解决了半加器的不足，可以加三个一位数（A，B，Cin，其中Cin是来自低位的进位输入）

它可以用两个半加器+一个OR门来实现

- 第一半加器:加A和B，得到临时Sum和$Carry_1$
- 第二半加器:加临时Sum和Cin，得到最终Sum和$Carry_2$
- 最终进位:Carry=$Carry_1$OR$Carry_2$

$Sum=A⊕B⊕Cin$ 

$Cout=(A⋅B)+(Cin⋅(A⊕B))$

