# Adder-UVM-Env

**Adder-UVM-Env** is a SystemVerilog UVM-based verification environment created to verify a simple **adder** design.  
It demonstrates a full UVM testbench structure — from sequence generation to scoreboard checking — using `uvm_pkg` and `uvm_macros`.

---

## Project Structure

| Component      | Description |
|----------------|-------------|
| **transaction** | UVM sequence item holding operands `a`, `b` and the sum `y`. |
| **generator**   | UVM sequence that randomizes operands and sends them to the driver. |
| **driver**      | Drives operands to the DUT interface (`add_if`) using transactions from the sequencer. |
| **monitor**     | Samples DUT interface signals and sends observed data to the scoreboard. |
| **scoreboard**  | Compares DUT output with expected sum to determine pass/fail. |
| **agent**       | Groups driver, monitor, and sequencer into a reusable verification unit. |
| **env**         | Top-level environment containing the agent and scoreboard. |
| **test**        | Configures and runs the environment, starting the generator. |
| **add_tb**      | Testbench module instantiating the DUT and launching the UVM test. |

---

## Verification Flow

1. **Generator** creates randomized input pairs (`a`, `b`).
2. **Driver** drives inputs to the DUT via `add_if`.
3. **Monitor** observes the inputs and DUT output `y`.
4. **Scoreboard** checks if `y == a + b`.
5. **UVM Reporting** prints "Test Passed" or "Test Failed" per transaction.

---

## Running the Testbench

### Prerequisites
- SystemVerilog simulator with UVM support (e.g., **QuestaSim**, **VCS**, or **Aldec Riviera-PRO**)
- `uvm_pkg` compiled and available in your simulator

### Compile and Run
```bash
# Example using QuestaSim
vlog +incdir+$UVM_HOME/src add_if.sv dut.sv adder_uvm_tb.sv
vsim -c -do "run -all" add_tb
