## Pt.1: Introduction

1. 编写ALU：例中为alu.sv，存在预期的行为了

2. 有`verilator --cc alu.sv`，Verilator将systemverilog转为cpp

3. 原文说得很清晰，是易于理解的：

   **转换结果**：

   运行上述命令会在我们的工作目录中生成一个名为`obj_dir`的新文件夹。这是我们所有转换后的源的去向：

   ```bash
   $ ls -l obj_dir/
   Valu___024unit.cpp  Valu___024unit__Slow.cpp  Valu.cpp  Valu.mk
   Valu__Syms.cpp  Valu__ver.d Valu___024unit.h    Valu_classes.mk
   Valu.h    Valu__Slow.cpp  Valu__Syms.h    Valu__verFiles.dat
   ```

   生成的`.mk`文件将用于`Make`构建我们的模拟可执行文件，**而`.h`和`.cpp`文件包含由 SystemVerilog 转换产生的 C++ 头文件和实现源。**（我建议你探索一下，看看你是否能弄清楚什么在哪里）

   其中：**Valu**是**V + alu**，而不是单词**valu** !!!

   PS:

   - **`Valu.h`- 这是主要设计标头，其中包含转换后的“ALU”*类*定义（class definition） —— 这是我们将在 C++ 测试平台中“实例化”为 DUT 的内容。**
   - `Valu___024unit.h`- 这是“ALU”类的内部标头，它包含我们的`operation_t`类型定义。

4. 编写**Testbench**：例中为tb_alu.cpp

   1. 如何理解dut和alu，DUT将ALU落地，是可以检验的，进而达到检验ALU的目的（**例化的理解**）

   2. 关于一些很不清楚的cpp语法。

      1. `Valu *dut = new Valu;` 这行代码是在 C++ 中使用 **`new` 运算符**来动态地分配内存并创建 `Valu` 类的新实例。在这种情况下，`dut` 变量将指向一个 `Valu` 类的对象。

      2. `dut` 是一个指向 `Valu` 对象的指针（所以我们用->算符），所以 `dut->trace(m_trace, 5);` 用于调用 `Valu` 对象的 `trace` 函数。至于为什么有参数m_trace，和函数trace的具体内容有关。现在母鸡。

      3. 现在大概可以说Pt.1看懂了。

         

5. 模拟可执行文件 Simulation Executable

   To build the simulation executable, we need to run Verilator **again** to regenerate the `.mk` files to include the C++ testbench - this is done using `--exe tb_alu.cpp`:

   ```bash
       $ verilator -Wall --trace -cc alu.sv --exe tb_alu.cpp
   ```

   为了构建我们的可执行文件，我们执行以下操作：

   ```bash
       $ make -C obj_dir -f Valu.mk Valu
   ```

   构建完成后，只需运行`Valu`二进制文件即可运行模拟：

   ```bash
   	$ ./obj_dir/Valu
   	$
   ```



## Pt.2: Basics of SystemVerilog verification using C++

1. Long commands are for troglodytes

   Makefile:

   ```makefile
   MODULE=alu
   
   .PHONY:sim
   sim: waveform.vcd 
   # ./obj_dir/V$(MODULE) 
   # make -C obj_dir -f V$(MODULE).mk V$(MODULE)
   # verilator -Wall --trace -cc $(MODULE).sv --exe tb_$(MODULE).cpp
   # The above three steps are what to do when inputting command "make sim". The left FOUR .PHONY are not that important having other functions.
   
   .PHONY:verilate
   verilate: .stamp.verilate 
   
   .PHONY:build
   build: obj_dir/Valu
   
   .PHONY:waves
   waves: waveform.vcd
   	@echo
   	@echo "### WAVES ###"
   	gtkwave waveform.vcd
   
   waveform.vcd: ./obj_dir/V$(MODULE)
   	@echo
   	@echo "### SIMULATING ###"
   	@./obj_dir/V$(MODULE)
   
   ./obj_dir/V$(MODULE): .stamp.verilate
   	@echo
   	@echo "### BUILDING SIM ###"
   	make -C obj_dir -f V$(MODULE).mk V$(MODULE)
   
   .stamp.verilate: $(MODULE).sv tb_$(MODULE).cpp
   	@echo
   	@echo "### VERILATING ###"
   	verilator -Wall --trace -cc $(MODULE).sv --exe tb_$(MODULE).cpp
   	@touch .stamp.verilate 
   #在这里，@ 符号用于告诉 Make 工具不要输出该命令的执行信息。因此，@touch .stamp.verilate 命令在 Makefile 中的这个上下文中被用来更新 .stamp.verilate 文件的时间戳，以指示 Verilog 编译已经完成。
   
   .PHONY:lint
   lint: $(MODULE).sv
   	verilator --lint-only $(MODULE).sv
   
   .PHONY: clean
   clean:
   	rm -rf .stamp.*;
   	rm -rf ./obj_dir
   	rm -rf waveform.vcd
   ```

2. 

   ```cpp
   void dut_reset (Valu *dut, vluint64_t &sim_time){
       dut->rst = 0;
       if(sim_time >= 3 && sim_time < 6){
           dut->rst = 1;
           dut->a_in = 0;
           dut->b_in = 0;
           dut->op_in = 0;
           dut->in_valid = 0;
       }
   }
   ```

   C++：

   在函数参数 `vluint64_t &sim_time` 中的 `&` 符号是 C++ 中的**引用符号**。这表示 `sim_time` 是一个引用类型的参数，即该参数是一个指向 `vluint64_t` 类型变量的引用（引用不同于指针）。

   主函数的调用：

   ```
   while (sim_time < MAX_SIM_TIME) {
       dut_reset(dut, sim_time); //如此调用
   
       dut->clk ^= 1;
       dut->eval();
       m_trace->dump(sim_time);
       sim_time++;
   }
   
   ```

3. Pt.2的最终版cpp:

   ```cpp
   #include <stdlib.h>
   #include <iostream>
   #include <cstdlib>
   #include <verilated.h>
   #include <verilated_vcd_c.h>
   #include "Valu.h"
   #include "Valu___024unit.h"
   
   #define MAX_SIM_TIME 300
   #define VERIF_START_TIME 7
   vluint64_t sim_time = 0;
   vluint64_t posedge_cnt = 0;
   
   void dut_reset (Valu *dut, vluint64_t &sim_time){
       dut->rst = 0;
       if(sim_time >= 3 && sim_time < 6){
           dut->rst = 1;
           dut->a_in = 0;
           dut->b_in = 0;
           dut->op_in = 0;
           dut->in_valid = 0;
       }
   }
   
   void check_out_valid(Valu *dut, vluint64_t &sim_time){
       static unsigned char in_valid = 0; //in valid from current cycle
       static unsigned char in_valid_d = 0; //delayed in_valid
       static unsigned char out_valid_exp = 0; //expected out_valid value
   
       if (sim_time >= VERIF_START_TIME) {
           out_valid_exp = in_valid_d;
           in_valid_d = in_valid;
           in_valid = dut->in_valid;
           if (out_valid_exp != dut->out_valid) {
               std::cout << "ERROR: out_valid mismatch, "
                   << "exp: " << (int)(out_valid_exp)
                   << " recv: " << (int)(dut->out_valid)
                   << " simtime: " << sim_time << std::endl;
           }
       }
   }
   
   void set_rnd_out_valid(Valu *dut, vluint64_t &sim_time){
       if (sim_time >= VERIF_START_TIME) {
           dut->in_valid = rand() % 2;
       }
   }
   
   int main(int argc, char** argv, char** env) {
       srand (time(NULL));
       Verilated::commandArgs(argc, argv);
       Valu *dut = new Valu;
   
       Verilated::traceEverOn(true);
       VerilatedVcdC *m_trace = new VerilatedVcdC;
       dut->trace(m_trace, 5);
       m_trace->open("waveform.vcd");
   
       while (sim_time < MAX_SIM_TIME) {
           dut_reset(dut, sim_time);
   
           dut->clk ^= 1;
           dut->eval();
   
           if (dut->clk == 1){
               dut->in_valid = 0;
               posedge_cnt++;
               set_rnd_out_valid(dut, sim_time);
               check_out_valid(dut, sim_time);
           }
   
           m_trace->dump(sim_time);
           sim_time++;
       }
   
       m_trace->close();
       delete dut;
       exit(EXIT_SUCCESS);
   }
   
   ```



## Pt.3: Traditional style verification example

1. Making the `operation_t` typedef available in the C++ testbench

   ```verilog
   typedef enum logic [1:0] {
        add     = 2'h1,
        sub     = 2'h2,
        nop     = 2'h0
   } operation_t /*verilator public*/;
   ```

   Notice the `/*verilator public*/` comment, after the `operation_t` name. **This tells verilator to convert this typedef to C++,** and make it publicly available - this happens during the [verilation (HDL to C++ conversion) step](https://www.itsembedded.com/dhd/verilator_1/#conversion-results). 

   If you look at the folder `obj_dir`, where our conversion artifacts are located, you will find `Valu___024unit.h`, **which contains the verilated version of our typedef enumeration:**

   ```cpp
       // TYPEDEFS
       // That were declared public
       enum operation_t {
           add = 1U,
           sub = 2U,
           nop = 0U
       };
   ```

   太细了老弟。

2. Traditional (time based)

   这是作者个人的叫法，这个语境下的traditional和time based如何理解？

   ```cpp
   if (posedge_cnt == 5){
       dut->in_valid = 1;       // assert in_valid on 5th cc
   }
   if (posedge_cnt == 7){
       if (dut->out_valid != 1) // check in_valid on 7th cc
           std::cout << "ERROR!" << std::endl;
   }
   ```



## Pt.4: Modern transactional (UVM) style C++ testbench

注：transactional事务型  UVM = Universal Verification Methodology 广泛使用的验证方法

<img src="https://itsembedded.com/static/dhd/verilator_4/alu_tb_transactional.jpg" alt="Verilator transactional UVM style testbench diagram itsembedded.com" style="zoom:80%;" />

按照这个模块来进行代码实现

1. AluInTx Transaction item

   ```cpp
   class AluInTx {
       public:
           uint32_t a;
           uint32_t b;
           enum Operation {
               add = Valu___024unit::operation_t::add,
               sub = Valu___024unit::operation_t::sub,
               nop = Valu___024unit::operation_t::nop
           } op;
   };
   
   ```

2. rndAluInTx Transaction item generator 

   ```cpp
   AluInTx* rndAluInTx(){
       //20% chance of generating a transaction
       if(rand()%5 == 0){
           AluInTx *tx = new AluInTx();
           tx->op = AluInTx::Operation(rand() % 3); // Our ENUM only has entries with values 0, 1, 2
           tx->a = rand() % 11 + 10; // generate a in range 10-20
           tx->b = rand() % 6;  // generate b in range 0-5
           return tx;
       } else {
           return NULL;
       }
   }
   
   ```

3. AluInDrv Input interface driver

   ```cpp
   class AluInDrv {
       private:
           Valu *dut;
       public:
           AluInDrv(Valu *dut){
               this->dut = dut;
           }
   
           void drive(AluInTx *tx){
               // we always start with in_valid set to 0, and set it to
               // 1 later only if necessary
               dut->in_valid = 0;
   
               // Don't drive anything if a transaction item doesn't exist
               if(tx != NULL){
                   if (tx->op != AluInTx::nop) {
                       // If the operation is not a NOP, we drive it onto the
                       // input interface pins
                       dut->in_valid = 1;
                       dut->op_in = tx->op;
                       dut->a_in = tx->a;
                       dut->b_in = tx->b;
                   }
                   // Release the memory by deleting the tx item
                   // after it has been consumed
                   delete tx;
               }
           }
   };
   
   ```

   **后略······**

