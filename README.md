# 3-to-8 Decoder using Verilog

## 📌 Project Overview

This project implements a **3-to-8 Decoder** using Verilog HDL.

A decoder is a combinational digital circuit that converts an n-bit binary input into one of 2^n possible output lines.

For a 3-bit input:

```text
2^3 = 8
```

Therefore, a 3-to-8 decoder has:

* 3 input lines
* 8 output lines
* 1 enable input

When enabled, exactly one output line becomes HIGH.

---

## 🎯 Objective

The objective of this project is to design and verify a 3-to-8 Decoder using Verilog HDL.

The project demonstrates:

* Decoder operation
* Binary-to-one-hot conversion
* Enable control
* Combinational logic
* Verilog `case` statements
* Testbench-based verification

---

## 🛠️ Technologies Used

* Verilog HDL
* VS Code
* Icarus Verilog
* ModelSim / QuestaSim
* GTKWave (optional)
* GitHub

---

## 📂 Project Structure

```text
3-to-8-Decoder/
│
├── README.md
├── src/
│   └── decoder_3to8.v
│
├── testbench/
│   └── tb_decoder_3to8.v
│
└── simulation/
    └── simulation_results.txt
```

---

## 🔢 Inputs and Outputs

| Signal | Width | Description            |
| ------ | ----: | ---------------------- |
| A      | 3-bit | Binary input           |
| EN     | 1-bit | Enable input           |
| Y      | 8-bit | One-hot decoded output |

---

## ⚙️ Truth Table

| EN | A     | Y          |
| -: | ----- | ---------- |
|  0 | XXX   | `00000000` |
|  1 | `000` | `00000001` |
|  1 | `001` | `00000010` |
|  1 | `010` | `00000100` |
|  1 | `011` | `00001000` |
|  1 | `100` | `00010000` |
|  1 | `101` | `00100000` |
|  1 | `110` | `01000000` |
|  1 | `111` | `10000000` |

---

## 🧠 Working Principle

The decoder converts a binary input into a one-hot output.

For example:

```text
A = 101
```

Binary `101` represents decimal `5`.

Therefore:

```text
Y5 = 1
```

and the output becomes:

```text
00100000
```

Only one output is HIGH.

---

## 🔌 Enable Input

The enable signal controls whether the decoder operates.

When:

```text
EN = 1
```

the decoder is active.

When:

```text
EN = 0
```

all outputs are LOW.

Example:

```text
EN = 0
A  = 101
```

Output:

```text
Y = 00000000
```

---

## 🧪 Testbench

The testbench verifies:

* All eight possible input combinations
* Enable HIGH
* Enable LOW
* One-hot output generation

A total of **10 test cases** are included.

---

## ▶️ Simulation Using Icarus Verilog

Open the terminal inside the project directory.

### Compile

```bash
iverilog -o decoder_sim src/decoder_3to8.v testbench/tb_decoder_3to8.v
```

### Run

```bash
vvp decoder_sim
```

---

## 📋 Expected Output

```text
================================================
             3-TO-8 DECODER TEST
================================================
 ENABLE | INPUT | OUTPUT
-----------------------------------------------
   1    |  000  | 00000001
   1    |  001  | 00000010
   1    |  010  | 00000100
   1    |  011  | 00001000
   1    |  100  | 00010000
   1    |  101  | 00100000
   1    |  110  | 01000000
   1    |  111  | 10000000
   0    |  000  | 00000000
   0    |  101  | 00000000
================================================
```

---

## 📚 Concepts Demonstrated

* Decoder
* Binary decoding
* One-hot encoding
* Enable signal
* Combinational logic
* Truth tables
* Verilog `case` statement
* Testbench development
* Functional verification

---

## 🚀 Applications

Decoders are used in:

* Memory address decoding
* CPU control units
* Instruction decoding
* Digital displays
* Demultiplexing
* Device selection
* Digital communication systems

---

## 🚀 Future Improvements

This project can be extended to:

* 2-to-4 Decoder
* 4-to-16 Decoder
* Cascaded decoder
* Parameterized decoder
* Priority decoder
* FPGA implementation

---

## 👩‍💻 Author

**Harshu**

B.Tech - Electronics and Communication Engineering

---

## 📄 License

This project is created for educational and academic purposes.
```verilog
`timescale 1ns/1ps

module decoder_3to8 (
    input  wire [2:0] a,
    input  wire       en,
    output reg  [7:0] y
);

    always @(*) begin

        // Default output
        y = 8'b00000000;

        if (en) begin

            case (a)

                3'b000: y = 8'b00000001;
                3'b001: y = 8'b00000010;
                3'b010: y = 8'b00000100;
                3'b011: y = 8'b00001000;
                3'b100: y = 8'b00010000;
                3'b101: y = 8'b00100000;
                3'b110: y = 8'b01000000;
                3'b111: y = 8'b10000000;

                default: y = 8'b00000000;

            endcase

        end

    end

endmodule
```
```verilog
`timescale 1ns/1ps

module tb_decoder_3to8;

    reg  [2:0] a;
    reg        en;

    wire [7:0] y;

    // Instantiate Design Under Test
    decoder_3to8 DUT (
        .a(a),
        .en(en),
        .y(y)
    );

    task test_input;
        input [2:0] test_a;
        input       test_en;

        begin
            a  = test_a;
            en = test_en;

            #10;

            $display(
                "ENABLE=%b | INPUT=%b | OUTPUT=%b",
                en, a, y
            );
        end
    endtask

    initial begin

        $display("================================================");
        $display("             3-TO-8 DECODER TEST");
        $display("================================================");
        $display(" ENABLE | INPUT | OUTPUT");
        $display("-----------------------------------------------");

        // Enable = 1

        test_input(3'b000, 1'b1);
        test_input(3'b001, 1'b1);
        test_input(3'b010, 1'b1);
        test_input(3'b011, 1'b1);
        test_input(3'b100, 1'b1);
        test_input(3'b101, 1'b1);
        test_input(3'b110, 1'b1);
        test_input(3'b111, 1'b1);

        // Enable = 0
        test_input(3'b000, 1'b0);
        test_input(3'b101, 1'b0);

        $display("================================================");

        $finish;

    end

endmodule
```
# 3-TO-8 DECODER SIMULATION RESULTS

## ENABLE | INPUT | OUTPUT

1    |  000  | 00000001
1    |  001  | 00000010
1    |  010  | 00000100
1    |  011  | 00001000
1    |  100  | 00010000
1    |  101  | 00100000
1    |  110  | 01000000
1    |  111  | 10000000
0    |  000  | 00000000
0    |  101  | 00000000

================================================
Simulation completed successfully.
==================================
