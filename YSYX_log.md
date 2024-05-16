# Day1: 24/3/4

### Record:

1. 开始阅读《跟我一起写Makefile》，正文9章，已读至第三章：**书写命令**
2. 近阶段的任务：
   1. 进一步认识verilator
   2. 学习verilog
   3. 预学习数电部分
   4. PA1实操

### Notes:

书中没讲十分清楚的地方：

1. `make -f Makefile1 -f Makefile2`

2. 
   在 Makefile 中，`:=` 是一种赋值操作符，表示使用“简单展开”的方式赋值。与之相对的，`=` 是使用“递归展开”的方式赋值。

   简单展开意味着变量的值在赋值时立即展开并且仅展开一次。而递归展开则意味着变量的值会在使用时被递归地展开。

   举个例子：

   ```
   # 使用 :=
   objects := $(wildcard *.c)
   
   # 使用 =
   sources = $(wildcard *.c)
   ```

   在这个例子中，如果目录中有 `foo.c` 和 `bar.c` 两个文件，那么在 `objects` 和 `sources` 变量中存储的值将是一样的。然而，它们的展开方式是不同的。

   - 在 `objects` 的赋值中，使用了 `:=`，这意味着 `$(wildcard *.c)` 会在赋值时被展开，因此 `objects` 变量的值会立即包含当前目录下所有以 `.c` 结尾的文件名。
   - 在 `sources` 的赋值中，使用了 `=`，这意味着 `$(wildcard *.c)` 会在使用 `sources` 变量的时候被展开，因此 `sources` 变量的值只有在使用时才会被计算。

# Day2: 24/3/5

### Record:

1. 《跟我一起写Makefile》读至CH06: **使用函数**

### Notes:

1. ```
   objects = program.o foo.o utils.o
   program : $(objects)
       cc -o program $(objects)
   
   $(objects) : defs.h
   ```

# Day3：24/3/6

### Record:

1. 《跟我一起写Makefile》剩下的部分浏览完

2. 双控灯写了，输出如下：

   `$ ./Vtop`
   `a = 1, b = 0, f = 1`
   `a = 1, b = 1, f = 0`
   `a = 1, b = 1, f = 0`
   `a = 0, b = 0, f = 0`
   `a = 1, b = 1, f = 0`
   `a = 0, b = 1, f = 1`
   `a = 0, b = 1, f = 1`
   `a = 1, b = 0, f = 1`
   `a = 0, b = 0, f = 0`
   `a = 0, b = 0, f = 0`

3. 接入了NVBOARD，双控灯目录现名为NVswitch。

4. 注释：此处双控的波形，真正意义上的NVBOARD的实现，都没有完成。

# Day4: 24/3/8

### Record:

1. 看完了B站视频：从电路设计的角度入门VerilogHDL
2. Vlab的Verilog语法教程，完整看完

### Notes:

Vlab中觉得值得记录下来的几句话：

1. always块内被赋值的信号应定义成reg类型，即使此信号在实际电路中是线型。
2. always语句实现组合逻辑应采用阻塞赋值方式。

# Day5: 24/3/9

### Record:

1. 读了nvboard的Makefile
2. PA1，RTFSC：day1 :
   1. build.mk添加了-E的部分，用处不大

### Notes:

1. grep -nr "\bmain\b" nemu/src

   这个命令会搜索 `nemu/src` 目录及其所有子目录中的文件，查找包含整个单词 "main" 的行，并输出匹配行的行号和文件名。

2. 所以应该先shell里grep函数，然后用ctags? //Nope, answer in 3.10

# Day6: 24/3/10

### Record:

1. ctags的使用
2. PA1， RTFSC: day2
3. 《Verilog数字系统设计教程》夏宇闻，读到 **初级篇**语法之五（目标：读完初级篇与中级篇）

### Notes (ctags) :

1. $ctags -R

2. :ptag say_hi(say_h为函数名，分屏看两个文件)  //  pc(pclose)关闭此窗口  //  vim中函数名分大小写

3. :tags  //看tags的列表

4. 组合技：

   1. /functionA  //将光标移至functionA
   2. **CTRL+]**  //整个移到要搜索的文件。草，不是[， 是]

5. :tags的列表  

   	1. :tag列表
   	2. 直接CTRL+t，列表往上
   	此时要向上移动的话，有':tag'，没有s

6. :ts  //列出所有匹配的标签tag

7. vim:

   `h：向左移动光标。`
   `j：向下移动光标。`
   `k：向上移动光标。`
   `l：向右移动光标。`
   `Ctrl + f：向下翻页。`
   `Ctrl + b：向上翻页。`
   `Ctrl + d：向下滚动半页。`
   `Ctrl + u：向上滚动半页。`

8. vim:

   `gg：移动到文件的开头。`
   `G：移动到文件的末尾`//用在.i文件最舒服

# Day7: 24/3/11

### Record:

1. 《Verilog数字系统设计教程》夏宇闻，读至中级篇，操，内容好多
1. PA1，RTFSC Day3......

### Notes:

1. getopt_long

# Day8: 24/3/12

### Record:

1. HDLbits至Vectors: Replication operator

### Notes:

1. Verilog中，或是|，不是+，易错!

2. 

   ```verilog
   wire [0:7] b;         // 8-bit wire where b[0] is the most-significant bit.
   ```

3. 大端法与小端法，结果还是不精通。写Verilog时遇到了问题。（见Day）

# Day9: 24/3/13

### Record:

1. HDbits至Modules: Carry-select adder 太草了。

### Notes:

1. 整理：

   1. ```verilog
      wire [0:7] b;         // 8-bit wire where b[0] is the most-significant bit.
      wire [7:0] b;         // 8-bit wire where b[7] is the most-significant bit
      //Build a circuit that sign-extends an 8-bit number to 32 bits. This requires a concatenation of 24 copies of the sign bit (i.e., replicate bit**[7]** 24 times) followed by the 8-bit number itself. HDbits中的一题。
      ```

2. vim: o  //光标到下一新空白行，且进入插入模式

# Day10: 24/3/14

### Record:

1. HDbits至Circuits

### Notes:

1. #### 精辟的，关于阻塞和非阻塞，与连续赋值：

   #### Blocking vs. Non-Blocking Assignment

   There are three types of assignments in Verilog:

   - **Continuous** assignments (`assign x = y;`). Can only be used when **not** inside a procedure ("always block").
   - Procedural **blocking** assignment: (`x = y;`). Can only be used inside a procedure.
   - Procedural **non-blocking** assignment: (`x <= y;`). Can only be used inside a procedure.

   In a **combinational** always block, use **blocking** assignments. In a **clocked** always block, use **non-blocking** assignments. 

   （block一是指**块**，如combinational（combinational对应clocked) always block；而是指阻塞，如blocking assignment）

2. **generate** 块允许你在编译时生成硬件描述，因此它适用于在设计中根据参数值生成可变数量的硬件结构。相比之下，**always**块用于描述电路的行为，它在仿真时执行，而不是在编译时。这意味着 always 块中的逻辑会在仿真过程中实时执行，而不是在编译时生成硬件。

3. 

# Day11: 24/3/17

### Record:

1. HDbits至Shift Registers

   #### Targets:

   1. 这周一定把HDbits看一遍
   2. 开始CSAPP的理论与实操，和数电实验

### Notes:

1. 无。

# Day12: 24/3/19

### Record:

1. HDbits至FSM

### Notes:

1. `=` 表示的是**阻塞赋值**，它会在当前赋值语句执行完毕之后，才会执行下一条语句。因此，如果同一个变量在同一个 `always` 块内被多次使用 `=` 进行赋值，**最后一次的赋值将会覆盖之前的赋值。**
2. `<=` 表示的是**非阻塞赋值**，它会在当前时钟周期结束之前被执行，而不会等待当前语句执行完毕。这意味着在同一个 `always` 块内使用 `<=` 进行赋值时，所有的赋值语句都会在同一时钟周期内被同时执行。因此，即使同一个变量在同一个 `always` 块内被多次使用 `<=` 进行赋值，**也不会造成覆盖，而是会并行执行。**

# Day13: 24/3/20

### Record:

1. HDbits，在做FSM

### Notes:

1. 无。

# Day14: 24/3/21

### Record:

1. HDbits, 推进了FSM，现在做完Lemming，到了Fsm_one-hot
2. 现在的打算，剩下的HDbits题抽做，但最后两个模块要做玩

### Notes:

1. vim中有替换！

   `:%s/OLD_CHAR/NEW_CHAR`

# Day15: 24/3/23

### Record:

1. CSAPP学习至第二章，先到浮点数

### Notes:

1. 

# Day16: 24/3/24

### Record:

1. CSAPP至第四章
1. HDbits的Fsm_ps2**时序图不清楚**（已解决）
1. 写了一些HDbits，详情见网站

### Notes:

1. CSAPP就不做详尽的笔记了，有：stack bottom 对应的是 high address
2. 函数调用：局部变量出现地址操作时要用栈保存而不是寄存器；在栈中，函数的参数需要8个字节对齐，而局部变量挨个存就行

# Day17: 24/3/25

### Record:

1. HDbits Testbench部分完成；HDbits计划上告一段落，剩下的题目详情见：https://hdlbits.01xz.net/wiki/Problem_sets

### Notes:

1. 夏宇闻书中，对于阻塞赋值与非阻塞赋值的使用整理了8个原则，比我的个人经验更为详尽充分：

   -    时序电路建模时，用非阻塞赋值。

   -    锁存器电路建模时，用非阻塞赋值。

   -    用always块建立组合逻辑模型时，用阻塞赋值。

   -    在同一个always块中建立时序和组合逻辑电路时，用非阻塞赋值。

   -    在同一个always块中不要既用非阻塞赋值又用阻塞赋值。

   -    不要在一个以上的always块中为同一个变量赋值。

   -    用$strobe系统任务来显示用非阻塞赋值的变量值。

   -    在赋值时不要使用#0延迟。

​	

# Day18: 24/3/26

### Record:

1. 看来CSAPP原书，至第三章的 控制
1. CSAPP视频至4.5流水线，草
1. 读了夏宇闻RISC-CPU部分内容
1. **周末做好准备，开始数电实验**

### Notes:

1. 无

# Day19,20: 24/3/31, 4/1

### Record:

1. NJU数电实验开始做了
2. nvboard使用不熟悉，第一个没用nvboard跑起来。读README，**其实已经说得很清楚了！！！**
3. verilator相关知识点懂得，但也不熟！很淦。

### Notes:

1. Makefile:

   1. 如果使用`=`赋值，会覆盖变量的旧值。而使用`+=`则会在现有值的基础上追加新值。

   2. `addprefix`是GNU Make中的一个函数，用于在一组字符串前面添加相同的前缀。其语法为：

      ```
      makefileCopy code
      $(addprefix prefix, names...)
      ```

      其中：

      - `prefix`是要添加的前缀字符串。
      - `names`是一个或多个要添加前缀的字符串。

   3. `abspath`将相对路径转为绝对路径

      `SRC_AUTO_BIND = $(abspath $(BUILD_DIR)/auto_bind.cpp)`

   4. 

   

# Day21: 24/4/2

### Record:

1. Itsembedded 看到**Pt.4**，但Pt.4还需再看，笔记见`C:\!CS_books&mk\Itsembedded_notes.md`

# Day22: 24/4/7

### Record:

1. 完成NJU的实验ex01
2. ex02开始了

### Notes:

# Day23,24: 24/4/10,11

### Record:

1. NJU数电实验ex02 ex03，ex03还在进行中

### Notes:

# Day25: 24/4/11

### Record:

1. ex03已完成，ex06进行中

### Notes:

1. 关于vim中的替换：

   `g` 是一个标志，表示全局替换。如果在替换命令结尾加上 `g`，则会在每一行中替换所有匹配到的文本，而不只是每一行的第一个匹配项。如果省略 `g` 标志，则只会替换每一行中第一个匹配项。


   在Vim中，你可以使用命令来指定区域内进行替换。下面是一些常用的替换命令：

   1. **替换当前行**：

      ```
      rubyCopy code
      :s/old_text/new_text/g
      ```

      这会在当前行中将所有的 `old_text` 替换为 `new_text`。

   2. **替换整个文件**：

      ```
      rubyCopy code
      :%s/old_text/new_text/g
      ```

      这会在整个文件中将所有的 `old_text` 替换为 `new_text`。

   3. **替换指定范围内的行**：

      ```
      bashCopy code
      :start_line_number,end_line_numbers/old_text/new_text/g
      ```

      这会在从 `start_line_number` 到 `end_line_number` 行之间的所有行中将 `old_text` 替换为 `new_text`。

   4. **替换当前选中的文本**： 首先按下 `v` 键，然后移动光标选择文本，然后输入 `:` 进入命令行模式，然后输入替换命令，例如：

      ```
      rubyCopy code
      :'<,'>s/old_text/new_text/g
      ```

      这会在当前选择的文本中将所有的 `old_text` 替换为 `new_text`。

2. 关于我直接在case中用xxxx:

   警告建议你考虑使用 `casex` 或 `casez` 语句，这样可以更明确地处理这些未知或不确定的值。`casex` 会匹配所有不确定的值，而 `casez` 只会匹配未知的值。

   如果你确信使用 `x` 或 `?` 是正确的，并且你不想要这个警告，你可以使用 Verilator 提供的指令来禁用警告，如下所示：

   ```ver
   verilogCopy code
   /* verilator lint_off CASEWITHX */
   ```

# Day26: 24/4/14

### Record:

1. ex06做完了
2. 任务有：看NJU DCexper网站实验**四五七**，其中实验七待做
3. **进度**：实验四看完，实验七要再看两遍，实验五未看

### Notes:

##### PS/2键盘控制器的设计：

1. 接收键盘送来的数据（有示例，键盘控制器）
2. 如何识别出按下的到底是什么按键
3. 如何显示出这些数据或键符

##### FPGA调试指导：

1. 前面的实验中，输入的器件一般是button和switch，他们很容易使用input array仿真模拟。本次实验需要自己编写仿真模型，模拟前文所述的键盘接口时序

   有1. 键盘仿真模型 2. 键盘测试代码

# Day27: 24/4/15

### Record:

1. 读完实验五
1. 实验七，差一个黑屏功能，快完成了
1. 实验八读完

### Notes:

##### 实验五 寄存器组及存储器：

存储器的初始化：

```verilog
initial
  begin
  ram[7] = 8'hf0; ram[6] = 8'h23; ram[5] = 8'h20; ram[4] = 8'h50;
  ram[3] = 8'h03; ram[2] = 8'h21; ram[1] = 8'h82; ram[0] = 8'h0D;
  end//方法一
```

当需要初始化的RAM数据量较大的时候，可以使用文件来在系统启动时直接装入RAM数据。 Verilog提供了以下语句来将文件中的数据导入到RAM中：

```verilog
initial
begin
  $readmemh("D:/digital_logic/mem1.txt", ram, 0, 7);
end//方法二
```

以上内容可以替代前例中的RAM初始化部分，将mem1.txt中的数据导入到ram变量的第0单元至第7单元。请注意，这里mem1.txt可以存在任何不包含中文字符的目录下，但是在初始化语句中一定要给出此文件的绝对路径，否则仿真时将看不到初始化数据。

mem1.txt的内容和格式如下：

```
@0 0d
@1 82
@2 21
@3 03
@4 20
@5 ff
@6 50
@7 04
```

其中@符号后为ram地址，随后是16进制的ram数据。在verilog中，$readmemh方法读取16进制数据，$readmemb方法读取2进制数据。

初始化存储器时可以选择存储器的部分单元进行初始化，其他单元不初始化。如，假设存储器ram有8个存储单元，下面的初始化表示只对存储器的 0$∼$5 号单元进行初始化，这也是可以的。

```verilog
initial
begin
  $readmemh("D:/digital_logic/mem1.txt", ram, 0, 5);
end
```



##### 使用IP核生成存储器:

Quartus提供了很多非常实用的IP核，利用这些IP核可以很方便的实现复杂的设计。我们可以使用**Quartus IP核** 来设计一个存储器，666



# Day28: 24/4/17

### Record:

1. NJU数电实验修改了前面的一个小错误，结束此部分
2. 提醒：C语言有几题没写
3. CSAPP第四章结束，流水线一些细节不是十分清楚，但结束

### Notes:

# Day29: 24/4/21

### Record:

1. 补C语言ex32双向链表
1. 再次着手PA

### Notes:

1. 学了一招，后面加了 -selection clipboard，可用范围更广

   ```bash
   cat filename | xclip -selection clipboard
   ```

2. getopt_long函数，还算清楚：

   https://blog.csdn.net/Dontla/article/details/121610581

# Day30,31: 24/4/24,27

### Record:

1. PA1

### Notes:

1. 初见sscanf：

   ```c
   #include <stdio.h>
   
   int main() {
       char str[] = "12345";
       int num;
   
       sscanf(str, "%d", &num);
       printf("Read number: %d\n", num);
   
       return 0;
   }
   ```

   这段代码会将字符串 "12345" 中的整数读取到变量 `num` 中，并打印出来。

2. 关于tmux用过多次但总是忘记，在此记录：

   1. **关闭会话：**

      ```bash
      exit
      ```

   2. **水平分割窗格：**

      ```
      Ctrl+b %
      ```

   3. **垂直分割窗格：**

      ```
      Ctrl+b "
      ```

   4. **切换窗格：**

      ```
      Ctrl+b 箭头键
      ```

   5. **调整窗格大小：**

      ```
      Ctrl+b Ctrl+箭头键
      ```

   6. **关闭当前窗格：**

      ```
      Ctrl+b x
      ```

   7. **重命名窗格：**

      ```
      Ctrl+b ,
      ```

   8. **在窗格之间切换：**

      ```bash
      Ctrl+b o
      ```

   tmux有换主题，键位有所调整，阅读~/.tmux.conf内容

3. 相应的有vim分屏也容易忘记：

   在 Vim 中分屏后，你可以使用以下方法在不同窗格之间切换光标所在位置：

   1. **使用键盘快捷键：**
      - 如果只有两个窗格，可以使用 `Ctrl+w` 命令配合方向键来切换窗格。
      - 如果有多个窗格，可以使用 `Ctrl+w` 命令配合 `h`, `j`, `k`, `l` 来切换到左、下、上、右窗格。
   2. **调整分屏大小**：
      - `Ctrl + w`，然后按下 `+` 或 `-`：增加或减少当前活动窗口的高度。
      - `Ctrl + w`，然后按下 `>` 或 `<`：增加或减少当前活动窗口的宽度。（尖括号）
      - `Ctrl + w`，然后按下 `=`：将所有窗口大小调整为相等。

4. vim中显示当前文件名：

   ```bash
   : echo expand ('%:p')
   ->/home/xzh/.ssh/ysyx-workbench/nemu/src/memory/paddr.c 
   : echo expand (%p)
   ->paddr.c
   ```

5. vim分屏：

   你想让其中一个窗口在全屏显示，以便更方便地复制内容，然后再恢复为分屏布局。在Vim中实现这一功能可以通过以下步骤：

   1. 移动光标到你想要暂时变为全屏的窗口。
   2. 输入命令 `Ctrl+w |` （按住 Ctrl 键，然后按下 w 键，再按下 | 键）。这会使得当前窗口充满整个 Vim 窗口的宽度。
   3. 进行你需要的复制操作。
   4. 恢复原有的分屏布局，输入命令 `Ctrl+w =` （按住 Ctrl 键，然后按下 w 键，再按下 = 键）。这会使得 Vim 重新调整窗口布局，使得各个窗口等宽等高。

   这样，你就可以在需要的时候将一个窗口暂时变为全屏，进行复制操作，然后再恢复原有的分屏布局。

# Day32: 24/5/5

### Record:

1. PA1可以看表达式求值讲义

### Notes:

1. 如果你想要将所选文件的绝对路径复制到剪贴板，以便后续使用，可以使用 `xclip` 。

   ```bash
   readlink -f $(fzf) | xclip -selection clipboard
   ```

两点（5/7）

1. get+prime 思路：

   ```c
   从右至左遍历：
   循环（如果变量仍为预置值，就继续循环）{
   首先是等于=不等!=，拿走
   如果有加减乘除，按加减和乘除的优先级赋值
   如果有左括号，就跳至左括号（继续遍历
   }
   //还是比较清楚的？
   ```

2. 括号多了一个左括号或者一个右括号的情况，似乎没有处理？

# Day33,34: 24/5/8,9

### Record:

1. PA1 expr做完，gen-expr在做。

2. gen-expr:

   - **.code.c: Function 'main'没改；**

   - **还没过滤掉被除数为0的情况；**

   - **tokens的值改了，buffer的值看样子也要改。**

### Notes:

1. 字符串赋值，死去的记忆：
   这样的语法没有错，但是需要注意的是，这将创建一个指向字符串常量 "hahuh" 的指针 `str`。在 C 语言中，字符串常量是不可变的，因此尝试修改 `str` 指向的字符串内容将导致未定义的行为。

   ```c
   char* str = "hahuh";
   ```

   这行代码会创建一个指针 `str`，它指向内存中字符串常量 "hahuh" 的地址。你可以使用这个指针来访问字符串的内容，但是不能通过 `str` 来修改字符串的内容。

   如果你想要创建一个可以修改的字符串，你应该使用字符数组，而不是指向字符串常量的指针。例如：

   ```c
   char str[] = "hahuh";
   ```

   这样就会创建一个可修改的字符串，因为它实际上是一个字符数组，而不是指向字符串常量的指针。

# Day35，36: 24/5/11，12

### Record:

1. gen-expr做起来比PA1主体部分要费时，今天5.11算是弄好了；
2. 利用gen-expr检测expr.c发现出错~5.12凌晨改完，也改了不少不断发现的新问题；
3. 凌晨也完成了watchpoint的第一个部分：拓展expr eval的功能。添加的几个有之前写好的，相当于只添加了一个逻辑与：&&;
4. 可以开始好好读watchpoint讲义了

### Notes:

1. vim系统剪切板：

   我已经安装了vim gdk，所以有：

   - `"+y`复制到系统剪切板中（解释一下：这里的+号不是表示”和y同时按下，按键的顺序应该是 shift **‘**、 shift =、 y）
   - `"+p`把系统粘贴板里的内容粘贴到vim（解释一下：这里的+号不是表示+和p同时按下，按键的顺序应该是 shift **‘**、shift =、 p）

   上面是快捷键的操作要怎么用呢，在vim正常模式下，按下 `v` 进入Visual模式，选取要复制的内容，然后按下 `"+y`，就复制到了系统剪切板中。如果粘贴到其他的打开的vim中就按下`"+p`，或者粘贴到其他文件或者网页中直接 `ctrl + v` 如果查看使用上面的命名复制是否成功呢？使用命令 `:reg`，会出现下面的界面，如果复制成功，”+ 后面的内容就是刚刚复制的，如果和你刚刚操作选中的内容一致，表示成功了

2. 关于free(): double free detected，有：https://stackoverflow.com/questions/67359233/what-does-this-error-means-free-double-free-detected-in-tcache-2

3. vim：将光标在 配对的括号间 移动

   在 Normal 模式下，将光标放在任意一个括号上，然后按下 `%` 键

4. Tpoara: 撤回撤销：`CTRL+Z`类似于vim的u，嘿嘿

5. 

# Day36: 24/5/13，14，15

### Record:

1. 5.13与5.14间的凌晨，写了一部分watchpoint的内容，添加了一点sdb之前留下的内容。
2. trace_and_difftest()没看。功能还没具体落地。
3. 5.15与5.16的凌晨，PA1大概写完了。
4. 读后感待补充。

### Notes:

# Day35: 24/11

### Record:

### Notes:

# Day35: 24/11

### Record:

### Notes:

# Day35: 24/11

### Record:

### Notes:

# Day35: 24/11

### Record:

### Notes:
