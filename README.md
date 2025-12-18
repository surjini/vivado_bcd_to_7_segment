## vivado_bcd_to_7_segment
## REG NUM :25015534
## NAME :Surjini.R
## EXPERIMENT – 2  Design and Implementation of BCD to 7-Segment Display Decoder

## AIM
To design and simulate a BCD to 7-segment display decoder using Verilog and display digits on the Boolean Board.

## TOOLS REQUIRED
Vivado Design Suite
Boolean Board (Spartan-7 FPGA)
USB Cable

## THEORY
A BCD-to-7-segment decoder converts a 4-bit BCD number into a pattern that lights up the 7-segment LED display.
The 7 segments are named:
a, b, c, d, e, f, g → represented as seg[6:0]
Example:
Digit 0 → 1000000 (segments lit except g)

## VERILOG PROGRAM
```
module bto7(
    input [3:0] bcd,
    output reg [6:0] seg,
    output reg [3:0] AN
);
    always @(*) begin
        AN = 4'b1110;  // enable rightmost display (active low)
        case(bcd)
            4'd0: seg = 7'b1000000;
            4'd1: seg = 7'b1111001;
            4'd2: seg = 7'b0100100;
            4'd3: seg = 7'b0110000;
            4'd4: seg = 7'b0011001;
            4'd5: seg = 7'b0010010;
            4'd6: seg = 7'b0000010;
            4'd7: seg = 7'b1111000;
            4'd8: seg = 7'b0000000;
            4'd9: seg = 7'b0010000;
            default: seg = 7'b1111111;
        endcase
    end
endmodule
```
## TESTBENCH
```
module tb_bto7;
    reg [3:0] bcd;
    wire [6:0] seg;

    bto7 dut (.bcd(bcd), .seg(seg));

    initial begin
        $display("Time\tBCD\tSegment");
        bcd = 0;

        repeat(16) begin
            #10;
            $display("%0t\t%b\t%b", $time, bcd, seg);
            bcd = bcd + 1;
        end

        $stop;
    end
endmodule
```
## XDC FILE (Boolean Board)
```
# 7-Segment Display Segments (D1)
set_property -dict {PACKAGE_PIN D7 IOSTANDARD LVCMOS33} [get_ports {seg[0]}]
set_property -dict {PACKAGE_PIN C5 IOSTANDARD LVCMOS33} [get_ports {seg[1]}]
set_property -dict {PACKAGE_PIN A5 IOSTANDARD LVCMOS33} [get_ports {seg[2]}]
set_property -dict {PACKAGE_PIN B7 IOSTANDARD LVCMOS33} [get_ports {seg[3]}]
set_property -dict {PACKAGE_PIN A7 IOSTANDARD LVCMOS33} [get_ports {seg[4]}]
set_property -dict {PACKAGE_PIN D6 IOSTANDARD LVCMOS33} [get_ports {seg[5]}]
set_property -dict {PACKAGE_PIN B5 IOSTANDARD LVCMOS33} [get_ports {seg[6]}]

# Digit Enable (AN)
set_property -dict {PACKAGE_PIN D5 IOSTANDARD LVCMOS33} [get_ports {AN[0]}]
set_property -dict {PACKAGE_PIN C4 IOSTANDARD LVCMOS33} [get_ports {AN[1]}]
set_property -dict {PACKAGE_PIN C7 IOSTANDARD LVCMOS33} [get_ports {AN[2]}]
set_property -dict {PACKAGE_PIN A8 IOSTANDARD LVCMOS33} [get_ports {AN[3]}]

# SWITCHES → BCD input
set_property -dict {PACKAGE_PIN V2 IOSTANDARD LVCMOS33} [get_ports {bcd[0]}]
set_property -dict {PACKAGE_PIN U2 IOSTANDARD LVCMOS33} [get_ports {bcd[1]}]
set_property -dict {PACKAGE_PIN U1 IOSTANDARD LVCMOS33} [get_ports {bcd[2]}]
set_property -dict {PACKAGE_PIN T2 IOSTANDARD LVCMOS33} [get_ports {bcd[3]}]
```
## PROCEDURE
Create new project in Vivado
Add Verilog files
Add constraints (XDC)
Simulate the design
Implement & generate bitstream
Program Boolean Board
Input BCD using switches → observe number on display

## OUTPUT 
<img width="1919" height="1083" alt="Screenshot 2025-12-18 105850" src="https://github.com/user-attachments/assets/ebabaab9-c884-4b1d-b3ab-fd8151952cde" />


## RESULT
The BCD to 7-segment decoder is successfully designed and implemented.
The Boolean board correctly displayed decimal digits (0–9) based on the BCD input from switches.
