# DataLab Note
> - 整数谜题​ 必须只使用直线代码（即无循环或条件判断）和一组受限的  C 算术和逻辑 运算符。具体来说，你只允许使用以下八个运算符：
  ! ~ & ^ | + << >>
  其中一些函数会进一步限制这个列表。此外，你不能使用任何长度超过 8 位的常量。详细的规则和关于期望的编码风格的讨论，请参阅 bits.c中的注释。
> - ​允许使用标准的控制结构（条件、循环），以及 int和 unsigned数据类型（包括任意的无符号和整数常量）。不允许使用任何联合体、结构体、数组，以及最重要的：不允许使用任何浮点数据类型、操作或常量。浮点操作数将以 unsigned类型传递给函数，返回值也应为 unsigned类型。你的代码应执行实现指定浮点运算的位操作。 

> 做的时候踩的最多的坑：**运算优先级**
  从高到低：
    括号​ ( )
    一元运算符​ ~!+(正) -(负) ++--等
    乘除类​ */%
    加减类​ +-
    移位​ << >>
    关系比较​ <<=>>=
    相等比较​ ==!=
    位与​ &
    位异或​ ^
    位或​ |
    逻辑与​ &&
    逻辑或​ ||
    条件运算符​ ? :
    赋值​ =
  尤其注意加减 > 移位、与或
- level 1
    1. 第一题考虑逻辑中的异或，只有T xor F 或者F xor T才能得到T，运用到二进制数上。
    ```c
    /* 
    * bitXor - x^y using only ~ and & 
    *   Example: bitXor(4, 5) = 1
    *   Legal ops: ~ &
    *   Max ops: 14
    *   Rating: 1
    */
    int bitXor(int x, int y) {
    int p=x&y;
    int q=~x&~y;
    return (~p)&(~q);
    }
    ```
- level 2
    1. 第一题看着很简单，其实卡了我有一段时间，最后想出了一个不那么完美的做法。`(x^(x+1)`只有x是Tmax和`0xffffffff`时才得到`0xffffffff`，排除掉后一种情况就好了。
    ```c
    /*
    * isTmax - returns 1 if x is the maximum, two's complement number,
    *     and 0 otherwise 
    *   Legal ops: ! ~ & ^ | +
    *   Max ops: 10
    *   Rating: 1
    */
    int isTmax(int x) {
    return !~((x^(x+1))+(!(x+1)));
    }
    ```
    2. 第二题比较简单，只需构造出`0xAAAAAAAA`，由于题目只允许最多八位二进制数，则利用`0xAA+(0xAA<<8)+(0xAA<<16)+(0xAA<<24)`构造，这里运算优先级要注意。
    ```c
    /* 
    * allOddBits - return 1 if all odd-numbered bits in word set to 1
    *   where bits are numbered from 0 (least significant) to 31 (most significant)
    *   Examples allOddBits(0xFFFFFFFD) = 0, allOddBits(0xAAAAAAAA) = 1
    *   Legal ops: ! ~ & ^ | + << >>
    *   Max ops: 12
    *   Rating: 2
    */
    int allOddBits(int x) {
    int a=0xAA+(0xAA<<8)+(0xAA<<16)+(0xAA<<24);
    int b=~a;
    return !~((x&a)+b);
    }
    ```
- level 3
    1. 第一题就很有难度，我的思路是把前7个字节和最后一个字节分开判断，`~((x&0xfffffff0)+0xffffffcf)`提取并判断前七字节是否等于0x3，若是，则转化为全0，`(((x&0xf)+0x6)&0xf0)`提取并判断最后一个字节是否为0\~9，这里用的方法是加上6看是否会进位，如果没进位则显然在0~9范围内，最后转化为全0。(操作符数量正好是15个)
    > 值得注意的是：必须用|而不能用+连接两个式子，因为+不能保证结果为0一定是两个0相加。
    ```c
    /* 
    * isAsciiDigit - return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0' to '9')
    *   Example: isAsciiDigit(0x35) = 1.
    *            isAsciiDigit(0x3a) = 0.
    *            isAsciiDigit(0x05) = 0.
    *   Legal ops: ! ~ & ^ | + << >>
    *   Max ops: 15
    *   Rating: 3
    */
    int isAsciiDigit(int x) {
    return !(~((x&0xfffffff0)+0xffffffcf)|(((x&0xf)+0x6)&0xf0));
    }
    ```
    2. 第二题很有思考性，我的思路是构造一个式子，当x非0时，便是y+0，当x是0时，便是0+z；于是得到了`(mask & y) | (~mask & z)`，而如何构造mask呢？我们的目的是x非0时，mask是全1，x为0时，mask是全0，于是`mask = (!!x << 31) >> 31`，!!能把任何非0数转化为1，而两次移位能把第0位的比特复制到所有位置上，完美实现目标！
    3. 第三题感觉相当有难度了。一开始很容易想到把y移到左边，即判断`x+(-y)≤0`，观察符号位。但有可能会发生溢出，导致判断出错。怎么解决呢？考虑会发生溢出的条件：x+y-或者x-y+,而这两种情况我们实际都不需要计算x+(-y)就能得出结论。因此，最后返回1的可能如下：`x-y+, x-y- && x≤y, x+y+ && x≤y`, 由于我们看的是符号位，无法判断等于的情况，因此把等于的情况单独列出，于是`x-y+, x-y- && x<y, x+y+ && x<y, x=y`。（这里的+其实是≥0）每个位置对应一个表达式$x_i$，如果用&连接这四个表达式，应满足如下需求：任何时候返回1时都只满足其中一个表达式，因此其中一个$x_i$=0时，其他$x$!=0。
    首先看x-y+，利用符号位很容易构造出`~(x>>31)|(y>>31)`不满足x-y+时，其值始终为全1。
    同理，x-y-构造`~(x>>31&y>>31)`，x+y+构造`x>>31|y>>31`，x=y构造`x^y`。
    对于x<y，我们构造`~((x+(~y+1))>>31)`，不发生溢出的情况下，不满足x<y时，其值始终为全1。
    接下来怎么实现`x-y- && x<y`？因为同时满足这两个条件时，这两个表达式都返回0，自然地，用|连接得到`~(x>>31&y>>31) | ~((x+(~y+1))>>31)`，只有同时满足两条件时才返回0。
    `x+y+ && x<y`同理，不做赘述。
    接下来用&连接四个表达式，并用!取反。
    可以看到，溢出影响不了整体式子，当x-y+时，若溢出，不管后面式子如何变化，我x-y+得到0，那必然返回1。当x+y-时，四个式子全是1，x<y的结果不重要。
    ```c
    /* 
    * isLessOrEqual - if x <= y  then return 1, else return 0 
    *   Example: isLessOrEqual(4,5) = 1.
    *   Legal ops: ! ~ & ^ | + << >>
    *   Max ops: 24
    *   Rating: 3
    */
    int isLessOrEqual(int x, int y) {
      int hx=x>>31,hy=y>>31;
      int xnyp=~hx|hy;
      int xpyp=(hx|hy);
      int xnyn=~(hx&hy);
      int xeqy=x^y;
      int le=~((x+(~y+1))>>31);
      return !((xnyn|le)&(xpyp|le)&xnyp&xeqy);
    }
    ```
- level 4
  1. 思路是把每一位的比特通过|运算集中到第0位，由于运算符号数目的限制，通过2的次方数来集中比特，最后通过^1来进行一个取反的操作。
  ```c
  //4
  /* 
  * logicalNeg - implement the ! operator, using all of 
  *              the legal operators except !
  *   Examples: logicalNeg(3) = 0, logicalNeg(0) = 1
  *   Legal ops: ~ & ^ | + << >>
  *   Max ops: 12
  *   Rating: 4 
  */
  int logicalNeg(int x) {
    int t1=x|x>>16;
    int t2=t1|t1>>8;
    int t3=t2|t2>>4;
    int t4=t3|t3>>2;
    int t5=t4|t4>>1;
    return (t5&1)^1;
  }
  ```
  2. 很难，我思路止步于将负数取反，找到最高位的那个1，但是我只能想到1位1位的右移来确定最高位1，这样操作符数目显然会超过90，想过用二分，但无从下手。这里借鉴了[fengmuzi](https://fengmuzi2003.gitbook.io/csapp3e)的解法，用的正是二分。
  ```c
  /* howManyBits - return the minimum number of bits required to represent x in
  *             two's complement
  *  Examples: howManyBits(12) = 5
  *            howManyBits(298) = 10
  *            howManyBits(-5) = 4
  *            howManyBits(0)  = 1
  *            howManyBits(-1) = 1
  *            howManyBits(0x80000000) = 32
  *  Legal ops: ! ~ & ^ | + << >>
  *  Max ops: 90
  *  Rating: 4
  */
  int howManyBits(int x) {
    int sign = x >> 31;
    int b16, b8, b4, b2, b1, b0;
    
    x = (sign & ~x) | (~sign & x);
    
    b16 = !!(x >> 16) << 4;
    x = x >> b16;
    b8 = !!(x >> 8) << 3;
    x = x >> b8;
    b4 = !!(x >> 4) << 2;
    x = x >> b4;
    b2 = !!(x >> 2) << 1;
    x = x >> b2;
    b1 = !!(x >> 1);
    x = x >> b1;
    b0 = x;
    
    return b16 + b8 + b4 + b2 + b1 + b0 + 1;
  }
  ```
  3. float第一题，不算难，首先分别提取符号位、指数、尾数，有几个注意事项：
  规格化数：exp!=0 && exp!=255
  非规格化数：exp=0
  无穷大：exp=255 && frac=0
  NaN：exp=255 && frac!=0
  如果是无穷大和NaN，直接返回自身就好了，
  如果是非规格化数，*2相当于把(exp<<23)|frac左移一位。
  如果是规格化数，*2相当于exp+1，注意如果exp+1后等于255，要返回无穷大。
  ```c
  //float
  /* 
  * floatScale2 - Return bit-level equivalent of expression 2*f for
  *   floating point argument f.
  *   Both the argument and result are passed as unsigned int's, but
  *   they are to be interpreted as the bit-level representation of
  *   single-precision floating point values.
  *   When argument is NaN, return argument
  *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
  *   Max ops: 30
  *   Rating: 4
  */
  unsigned floatScale2(unsigned uf) {
    int sign=uf>>31;
    int exp=(uf>>23)&0xff;
    int frac=uf&0x7fffff;
    if(exp==255) return uf;
    if(exp==0){
      int ef=exp<<23|frac;
      ef=ef<<1;
      return sign<<31|ef;
    }
    exp+=1;
    if(exp==255){
      frac=0;
    }
    return (sign<<31)|(exp<<23)|frac;
  }
  ```
  4. 不算难，有些地方比较绕，E>=31时必定溢出，E<31时对于规格化数，向零舍入需要注意一下。
  ```c
  /* 
  * floatFloat2Int - Return bit-level equivalent of expression (int) f
  *   for floating point argument f.
  *   Argument is passed as unsigned int, but
  *   it is to be interpreted as the bit-level representation of a
  *   single-precision floating point value.
  *   Anything out of range (including NaN and infinity) should return
  *   0x80000000u.
  *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
  *   Max ops: 30
  *   Rating: 4
  */
  int floatFloat2Int(unsigned uf) {
    int sign=uf>>31;
    int exp=(uf>>23)&0xff;
    int frac=uf&0x7fffff;
    int E=0,t=0;
    if(exp==255){
      return 0x80000000u;
    } else if(exp==0){
      E=-126;
    } else{
      E=exp-127;
    }
    if(E<0) return 0;
    if(E>=31) return 0x80000000u;
    t=1<<23|frac;
    if(E>23){
      t=t<<(E-23);
    } else{
      t=t>>(23-E);
    }
    return sign ? -t : t;
  }
  ```
  5. 不难，需要注意的是非规格化数怎么转化
  ```c
  /* 
  * floatPower2 - Return bit-level equivalent of the expression 2.0^x
  *   (2.0 raised to the power x) for any 32-bit integer x.
  *
  *   The unsigned value that is returned should have the identical bit
  *   representation as the single-precision floating-point number 2.0^x.
  *   If the result is too small to be represented as a denorm, return
  *   0. If too large, return +INF.
  * 
  *   Legal ops: Any integer/unsigned operations incl. ||, &&. Also if, while 
  *   Max ops: 30 
  *   Rating: 4
  */
  unsigned floatPower2(int x) {
    int exp=0,shift=0;
      if(x<-149){
        return 0;
      } else if(x>127){
        return 0x7F800000;
      } else if(x>=-126){
        exp=x+127;
        return exp<<23;
      } else{
        shift=-126-x;
        return 1<<23>>shift;
      }
  }
  ```