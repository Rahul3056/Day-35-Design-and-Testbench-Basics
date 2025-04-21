Here is the detailed explanation for:

---

### **Day 35: Design and Testbench Basics**

---

#### **1. Overview**

In SystemVerilog (and digital design in general), there are two major components to any verification environment:

1. **Design (RTL)**: Represents the actual hardware logic to be implemented.
2. **Testbench**: Provides stimuli to the design, observes the outputs, and verifies correctness.

A clear separation between the design and testbench code is essential. The design is intended to be synthesized into hardware, while the testbench is used only in simulation.

---

#### **2. RTL Design Basics**

RTL (Register Transfer Level) modeling defines digital logic using registers (flip-flops) and combinational logic.

**Key components of RTL design:**

- **Modules**: Encapsulate logic with input and output ports.
- **Procedural blocks**: `always_comb`, `always_ff`, etc.
- **Data types**: `logic`, `bit`, `int`, arrays
- **Continuous assignments**: `assign`
- **Finite State Machines (FSMs)**: Control flow logic

**Example:**

```systemverilog
module and_gate (
    input  logic a,
    input  logic b,
    output logic y
);
    assign y = a & b;
endmodule
```

---

#### **3. Testbench Basics**

A testbench is a **non-synthesizable** environment written to apply inputs to the DUT (Design Under Test) and check its outputs.

**Basic testbench structure includes:**

1. **Instantiation** of the DUT
2. **Initial block** for driving inputs
3. **Display or monitor statements** to observe outputs
4. **Delays or clock generation**
5. Optional: tasks, functions, and classes for better organization

**Example:**

```systemverilog
module tb_and_gate;
    logic a, b, y;

    // Instantiate the design
    and_gate dut (
        .a(a),
        .b(b),
        .y(y)
    );

    initial begin
        // Apply input combinations
        a = 0; b = 0; #10;
        a = 0; b = 1; #10;
        a = 1; b = 0; #10;
        a = 1; b = 1; #10;

        $display("Simulation complete");
        $finish;
    end
endmodule
```

---

#### **4. Design vs Testbench: Key Differences**

| Feature              | Design Code (RTL)       | Testbench Code              |
|---------------------|-------------------------|-----------------------------|
| Synthesizable        | Yes                     | No                          |
| Uses `initial` block | No                      | Yes                         |
| Blocking (`=`)       | Only in `always_comb`   | Common                      |
| Non-blocking (`<=`)  | Used in `always_ff`     | Rare unless modeling sequential delay |
| Purpose              | Hardware implementation | Verification of logic       |
| Assertions           | Rare                    | Common                      |
| Randomization        | Not used                | Common in testbenches       |

---



#### **6. Guidelines for Good RTL Design**

- Use `logic` instead of `reg`/`wire`
- Use `always_ff` for sequential logic
- Use `always_comb` for combinational logic
- Reset all flip-flops properly
- Avoid latches unless explicitly intended
- Keep clock and reset signals clean and well-defined

---

#### **7. Guidelines for Good Testbench Coding**

- Use `initial` blocks to define test stimulus
- Group repetitive tasks into reusable functions/tasks
- Use `#` delays or clocks to simulate time passage
- Monitor signals using `$display`, `$monitor`, or waveform tools
- Isolate testbench code in a separate module from design
- In advanced verification, use classes, randomization, and assertions

---

#### **8. Common Pitfalls**

- Driving a signal from both design and testbench
- Using non-synthesizable constructs in design code
- Forgetting to initialize inputs in testbench
- Not checking outputs in testbench (no verification)
- Using blocking assignments (`=`) in sequential logic
- Creating unintended latches in design

---

#### **9. Summary**

- RTL design models hardware logic and must be synthesizable.
- A testbench is written to stimulate and verify the DUT behavior in simulation.
- Use specific SystemVerilog constructs (`always_comb`, `always_ff`) for better reliability and clarity.
- Always separate design and testbench code to maintain modularity and correctness.
