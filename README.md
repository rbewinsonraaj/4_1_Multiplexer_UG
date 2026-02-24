# 4_1_Multiplexer
# EXP NO: 1.A SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER
# AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

# APPARATUS REQUIRED
Vivado 2023.1

# Procedure
1.Open Vivado 2023.1.
2.Create a New RTL Project and give a name (e.g., Mux4_to_1).
3.Add/create your Verilog files and testbench.
4.Select an FPGA part (e.g., xc7a35ticsg324-1L).
5.Run Synthesis to check for errors.
6.Run Simulation → Run Behavioral Simulation.
7.Observe the waveforms of inputs and outputs.
8.Adjust simulation time if needed (e.g., 1000ns).
9.Save the project and take screenshots of results.
10.Close simulation.

# Logic Diagram

<img width="614" height="424" alt="Screenshot 2026-02-11 195225" src="https://github.com/user-attachments/assets/03cabe3f-914c-4163-bea7-2ba5257ed5a7" />


# Truthtable 

<img width="496" height="376" alt="Screenshot 2026-02-11 194234" src="https://github.com/user-attachments/assets/3530b20d-cc75-480c-bc14-d10b21a36376" />


# Verilog Code
4:1 MUX Gate-Level Implementation
```
module mux4_gate (
    input  wire I0, I1, I2, I3,
    input  wire S0, S1,
    output wire Y
);
    wire S0_bar, S1_bar;
    wire w0, w1, w2, w3;

    not (S0_bar, S0);
    not (S1_bar, S1);

    and (w0, I0, S1_bar, S0_bar);
    and (w1, I1, S1_bar, S0);   
    and (w2, I2, S1, S0_bar);    
    and (w3, I3, S1, S0);    

    or (Y, w0, w1, w2, w3);

endmodule
```
4:1 MUX Gate-Level Implementation- Testbench
```
module tb_mux4_gate;
reg I0, I1, I2, I3;
reg S0, S1;
wire Y;

mux4_gate uut (I0,I1,I2,I3,S0,S1,Y);
initial
begin
    I0 = 0; I1 = 0; I2 = 0; I3 = 0;S0 = 0; S1 = 0;
    #10 I0 = 1; I1 = 0; I2 = 0; I3 = 0; S1 = 0; S0 = 0;
    #10 I0 = 0; I1 = 1; I2 = 0; I3 = 0; S1 = 0; S0 = 1;
    #10 I0 = 0; I1 = 0; I2 = 1; I3 = 0; S1 = 1; S0 = 0;
    #10 I0 = 0; I1 = 0; I2 = 0; I3 = 1; S1 = 1; S0 = 1;
end
endmodule
```
# Simulated Output Gate Level Modelling
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e6b31c36-868b-4e29-8d50-c0cfa916444a" />


4:1 MUX Data flow Modelling
```
module mux4_dataflow (
    input  wire I0, I1, I2, I3,
    input  wire S0, S1,
    output wire Y
);
    assign Y = (~S1 & ~S0 & I0) |(~S1 &  S0 & I1) | S1 & ~S0 & I2) | ( S1 &  S0 & I3);
endmodule

```
4:1 MUX Data flow Modelling- Testbench
```
module tb_mux4_gate;
reg I0, I1, I2, I3;
reg S0, S1;
wire Y;

mux4_gate uut (I0, I1, I2, I3,S0, S1,Y);

initial
begin
    I0 = 0; I1 = 0; I2 = 0; I3 = 0;
    S0 = 0; S1 = 0;
    #10 I0 = 1; I1 = 0; I2 = 0; I3 = 0; S1 = 0; S0 = 0;
    #10 I0 = 0; I1 = 1; I2 = 0; I3 = 0; S1 = 0; S0 = 1;
    #10 I0 = 0; I1 = 0; I2 = 1; I3 = 0; S1 = 1; S0 = 0;
    #10 I0 = 0; I1 = 0; I2 = 0; I3 = 1; S1 = 1; S0 = 1;
    #10 $stop;
end
endmodule
```
# Simulated Output Dataflow Modelling
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/62e5d650-4847-4703-87fa-13a3ead5e66a" />


4:1 MUX Behavioral Implementation
```
module mux4_to_1_behavioral (
    input wire A,
    input wire B,
    input wire C,
    input wire D,
    input wire S0,
    input wire S1,
    output reg Y
);
    always @(*) begin
        case ({S1,S0})
            2'b00: Y = A;  
            2'b01: Y = B;   
            2'b10: Y = C;   
            2'b11: Y = D;   
            default: Y = 0;
        endcase
    end
endmodule
```
#4:1 MUX Behavioral Modelling- Testbench
```
module tb_mux4_to_1_behavioral;
reg A, B, C, D;
reg S0, S1;
wire Y;

mux4_to_1_behavioral uut (A,B,C,D,S0,S1,Y);
initial 
begin
    A = 0; B = 0; C = 0; D = 0;
    S0 = 0; S1 = 0;
    #10 A = 1; B = 0; C = 0; D = 0; S1 = 0; S0 = 0;
    #10 A = 0; B = 1; C = 0; D = 0; S1 = 0; S0 = 1;
    #10 A = 0; B = 0; C = 1; D = 0; S1 = 1; S0 = 0;
    #10 A = 0; B = 0; C = 0; D = 1; S1 = 1; S0 = 1;
    #10 $stop;
end
endmodule
```
# Simulated Output Behavioral Modelling
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/1074a349-2e9b-472b-9d7f-590715c00283" />


#4:1 MUX Structural Implementation
```
module mux2_to_1 (
    input wire A,
    input wire B,
    input wire S,
    output wire Y
);
    assign Y = S ? B : A;
endmodule

module mux4_to_1_structural (
    input wire A,
    input wire B,
    input wire C,
    input wire D,
    input wire S0,
    input wire S1,
    output wire Y
);
    wire w1, w2;
    mux2_to_1 M1 (.A(A), .B(B), .S(S0), .Y(w1));
    mux2_to_1 M2 (.A(C), .B(D), .S(S0), .Y(w2));
    mux2_to_1 M3 (.A(w1), .B(w2), .S(S1), .Y(Y));

endmodule
```
# Testbench Implementation
```
module tb_mux4_to_1_structural;
reg A, B, C, D;
reg S0, S1;
wire Y;

mux4_to_1_behavioral uut (A,B,C,D,S0,S1,Y);
initial 
begin
    A = 0; B = 0; C = 0; D = 0;
    S0 = 0; S1 = 0;
    #10 A = 1; B = 0; C = 0; D = 0; S1 = 0; S0 = 0;
    #10 A = 0; B = 1; C = 0; D = 0; S1 = 0; S0 = 1;
    #10 A = 0; B = 0; C = 1; D = 0; S1 = 1; S0 = 0;
    #10 A = 0; B = 0; C = 0; D = 1; S1 = 1; S0 = 1;
    #10 $stop;
end
endmodule
```
# Simulated Output Structural Modelling
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/16db35d0-35dd-47b2-9050-33745ec49b5d" />


# CONCLUSION
In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.
