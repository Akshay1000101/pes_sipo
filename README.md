# pes_sipo

## Serial in Parallel out Shift Register

The shift register, which allows serial input (one bit after the other through a single data line) and produces a parallel output is known as the Serial-In Parallel-Out shift register. The logic circuit given below shows a serial-in-parallel-out shift register. The circuit consists of four D flip-flops which are connected. The clear (CLR) signal is connected in addition to the clock signal to all 4 flip flops in order to RESET them. The output of the first flip-flop is connected to the input of the next flip flop and so on. All these flip-flops are synchronous with each other since the same clock signal is applied to each flip-flop. 
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/sipoimage.png?raw=true)
The above circuit is an example of a shift right register, taking the serial data input from the left side of the flip-flop and producing a parallel output. They are used in communication lines where demultiplexing of a data line into several parallel lines is required because the main use of the SIPO register is to convert serial data into parallel data.

the code is
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/code%20new%20correct.PNG?raw=true)
First read the design and testbench using the command

iverilog pes_sipo.v pes_sipo_tb.v

./a.out

then open the waveform using gtkwave
gtkwave sipo.vcd
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/waveform.PNG?raw=true)
Then synthesize the design and generate the netlist file
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog pes_sipo.v
synth -top pes_sipo
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr pes_sipo_net.v
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/yosys%201.PNG?raw=true)
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/yosys%202.PNG?raw=true)
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/yosys%203.PNG?raw=true)

### Gate Level simulation
iverilog primitives.v sky130_fd_sc_hd.v pes_sipo_net.v pes_sipo_tb.v
gtkwave pes_sipo.vcd
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/gate%20level%20simulation.PNG?raw=true)

The Synthesis and Simulation Waveforms are matching