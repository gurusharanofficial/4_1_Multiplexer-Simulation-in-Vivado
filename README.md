# SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure

1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** and give a name (e.g., `Mux4_to_1`).  
3. Add/create your Verilog files and testbench.  
4. Select an FPGA part (e.g., `xc7a35ticsg324-1L`).  
5. Run **Synthesis** to check for errors.  
6. Run **Simulation** → **Run Behavioral Simulation**.  
7. Observe the waveforms of inputs and outputs.  
8. Adjust simulation time if needed (e.g., 1000ns).  
9. Save the project and take screenshots of results.  
10. Close simulation.  

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation
```verilog
module mux4to1_gl(I,S,y);
input [3:0]I;
input [1:0]S;
output y;
wire [3:0]w;
and g1(w[0],I[0],~S[0],~S[1]);
and g2(w[1],I[1],~S[0],S[1]);
and g3(w[2],I[2],S[0],~S[1]);
and g4(w[3],I[3],S[0],S[1]);
or g5(y,w[0],w[1],w[2],w[3]);
endmodule
```
### 4:1 MUX Gate-Level Implementation- Testbench
```verilog
`timescale 1ns/1ps
module mux4to1_gl_tb;
reg [3:0]I;
reg[1:0]S;
wire y;
mux4to1_gl uut(I,S,y);
initial
begin
I=4'b0111;
S=2'b00;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],y);
S=2'b01;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],y);
S=2'b10;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],y);
S=2'b11;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],y);
$finish;
end
endmodule
```
## Simulated Output Gate Level Modelling

<img width="1916" height="1195" alt="Screenshot 2025-09-17 135445" src="https://github.com/user-attachments/assets/0eb525cb-e10b-49e7-a1a5-115eb8c8802b" />

---
### 4:1 MUX Data flow Modelling
```verilog
module MUX4_1(I,s,Y);
input [3:0]I;
input [1:0]s;
output Y;
wire [4:1]w;
assign w[1]=(~s[1])&&(~s[0])&&I[0];
assign w[2]=(~s[1])&&(s[0])&&I[1];
assign w[3]=(s[1])&&(~s[0])&&I[2];
assign w[4]=(s[1])&&(s[0])&&I[3];
assign Y=w[1]||w[2]||w[3]||w[4];
endmodule
```
### 4:1 MUX Data flow Modelling- Testbench
```verilog
module MUX4_1_tb;
reg [3:0]I;
reg [1:0]s;
wire Y;
MUX4_1 uut(I,s,Y);
initial 
begin
I=4'b0111;
s=2'b00;
#10;
$display("selection is %b %b, output : %b",s[0],s[1],Y);
s=2'b01;
#10;
$display("selection is %b %b, output : %b",s[0],s[1],Y);
s=2'b10;
#10;
$display("selection is %b %b, output : %b",s[0],s[1],Y);
s=2'b11;
#10;
$display("selection is %b %b, output : %b",s[0],s[1],Y);
$finish;
end 
endmodule
```
## Simulated Output Dataflow Modelling

<img width="1895" height="1195" alt="image" src="https://github.com/user-attachments/assets/f79b1fc2-7180-4cf9-b3e4-46a2ed0f3145" />

---
### 4:1 MUX Behavioral Implementation
```verilog
module MUX_4_1(I,S,Y);
input [3:0]I;
input [1:0]S;
output reg Y;
always @(I,S)
      begin
         case(S)
         2'b00:Y=I[0];
         2'b01:Y=I[1];
         2'b10:Y=I[2];
         2'b11:Y=I[3];
         endcase
      end   
endmodule
```
### 4:1 MUX Behavioral Modelling- Testbench
```verilog
`timescale 1ns/1ps
module MUX_4_1tb;
reg [3:0]I;
reg[1:0]S;
wire Y;
MUX_4_1 uut(I,S,Y);
initial
begin
I=4'b1001;
S=2'b00;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
S=2'b01;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
S=2'b10;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
S=2'b11;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
$finish;
end
endmodule
```
## Simulated Output Behavioral Modelling

![Uploading Screenshot 2025-09-17 082550.png…]()



### 4:1 MUX Structural Implementation

![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)


```verilog
module mux2to1 (Y, I0, I1, S);
  input I0, I1, S;
  output Y;
  wire w1, w2, w3;

  not (w1, S);
  and (w2, I0, w1);
  and (w3, I1, S);
  or  (Y, w2, w3);
endmodule

module mux4to1 (Y, I, S);
  output Y;
  input [3:0] I;
  input [1:0] S;
  wire w1, w2;
  mux2to1 m1(w1, I[0], I[1], S[0]);
  mux2to1 m2(w2, I[2], I[3], S[0]);
  mux2to1 m3(Y, w1, w2, S[1]);
endmodule

```
### Testbench Implementation
```verilog
module mux4to1_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
mux4to1 uut(Y,I,S);
initial
begin
I=4'b0111;
S=2'b00;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
S=2'b01;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
S=2'b10;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
S=2'b11;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
$finish;
end
endmodule
```
## Simulated Output Structural Modelling
<img width="1919" height="1199" alt="Screenshot 2025-09-17 134221" src="https://github.com/user-attachments/assets/dec4d32f-0c29-40e2-b974-e3391e7700c1" />

---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.

