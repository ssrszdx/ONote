# IEEE浮点数表示--规格化非规格化无穷大NaN

‍

1.规格化的值

以sizeof(float)=4为例：

1.5的[浮点数](https://so.csdn.net/so/search?q=%E6%B5%AE%E7%82%B9%E6%95%B0&spm=1001.2101.3001.7020)表示：

1)1.5转换为2[进制](https://so.csdn.net/so/search?q=%E8%BF%9B%E5%88%B6&spm=1001.2101.3001.7020)：1.1

2)转换：0.1*2^0 （整数部分的1省略）

3)得到阶码：127+0=127，即0111 1111 (指数部分可能是负数，为了兼容负数，需要+127)

4)得到尾数：1，后面补齐0

5)确定符号位：0

所以，1.5的浮点数表示如下：

|符号位:1bit|阶码:8bits|尾数:23bits|
| -------------| ------------| ------------------------------|
|0|0111 1111|1000 0000 0000 0000 0000 000|

程序验证如下：

**[cpp]**  [view plain](http://blog.csdn.net/hqin6/article/details/6701109# "view plain") [copy](http://blog.csdn.net/hqin6/article/details/6701109# "copy")

1. **#include &lt;stdio.h&gt;**
2. **#include &lt;stdlib.h&gt;**
3. ‍
4. **int** main()
5. {
6. **float** f = 1.5;
7. printf(**&quot;%x&quot;**, *(*​***int***)&f);**//打印3fc00000**
8. **return** 0;
9. }

3fc00000即：

0  01111111  1000  0000 0000 0000 0000 000

一致！

2.非规格化的值

即，所有的阶码都是0

|符号位:1bits|阶码:8bits|尾数:23bits|
| --------------| ------------| ------------------------------|
|x|0000 0000|xxxx xxxx xxxx xxxx xxxx xxx|

用途有2：

1)提供了一种表示值0的方法

2)表示那些非常接近于0.0的数，对“逐渐溢出”属性的支持

代码示例：

**[cpp]**  [view plain](http://blog.csdn.net/hqin6/article/details/6701109# "view plain") [copy](http://blog.csdn.net/hqin6/article/details/6701109# "copy")

1. **小端模式机器实验**
2. **#include &lt;stdio.h&gt;**
3. **#include &lt;stdlib.h&gt;**
4. **typedef** **struct** _float
5. {
6. **int** w:23;
7. **int** j:8;
8. **int** s:1;
9. }Float;
10. **int** main()
11. {
12. **float** f = 0;
13. Float obj;
14. obj.s = 0;
15. obj.j = 0;
16. obj.w = 0x400000;
17. f = *(*​***float***)(&obj);
18. printf(**&quot;%f&quot;**, f);**//输出0.000000**
19. **return** 0;
20. }

# 3.[无穷大](https://so.csdn.net/so/search?q=%E6%97%A0%E7%A9%B7%E5%A4%A7&spm=1001.2101.3001.7020)

|符号位:1bits|阶码:8bits|尾数:23bits|
| --------------| ------------| ------------------------------|
|x|1111 1111|0000 0000 0000 0000 0000 000|

代码示例：

**[cpp]**  [view plain](http://blog.csdn.net/hqin6/article/details/6701109# "view plain") [copy](http://blog.csdn.net/hqin6/article/details/6701109# "copy")

1. **#include &lt;stdio.h&gt;**
2. **#include &lt;stdlib.h&gt;**
3. **typedef** **struct** _float
4. {
5. **int** w:23;
6. **int** j:8;
7. **int** s:1;
8. }Float;
9. **int** main()
10. {
11. **float** f = 0;
12. Float obj;
13. obj.s = 0;
14. obj.j = 0xff;
15. obj.w = 0x0;
16. f = *(*​***float***)(&obj);
17. printf(**&quot;%f&quot;**, f);**//输出inf**
18. **return** 0;
19. }

# 4.NaN

|符号位:1bit|阶码:8bits|尾数:23bits|
| -------------| ------------| -------------|
|x|1111 1111|非全0|

代码示例：

**[cpp]**  [view plain](http://blog.csdn.net/hqin6/article/details/6701109# "view plain") [copy](http://blog.csdn.net/hqin6/article/details/6701109# "copy")

1. **#include &lt;stdio.h&gt;**
2. **#include &lt;stdlib.h&gt;**
3. **typedef** **struct** _float
4. {
5. **int** w:23;
6. **int** j:8;
7. **int** s:1;
8. }Float;
9. **int** main()
10. {
11. **float** f = 0;
12. Float obj;
13. obj.s = 0;
14. obj.j = 0xff;
15. obj.w = 0x1;
16. f = *(*​***float***)(&obj);
17. printf(**&quot;%f&quot;**, f);**//输出nan**
18. **return** 0;
19. }
