# CIRCOM 101

## Getting Started

To create a new `package.json`, run the following command:

```bash
npm init -y
```

Next, install **circom2** and **snarkjs** using npm:

```npm install circom2 snarkjs```

### Creating the Circuit

Let's create the `circuit.circom` file, which contains the circuit's logic.

### Compiling the Circuit
Compile the circuit using the following command:

```npx circom2 circuit.circom --r1cs --wasm```

Here:

```circuit.circom is the circuit file name.
--r1cs specifies the circuit type as R1CS.
--wasm is the output type
```
It which generates a new folder containing a **JavaScript file** for this circuit(wasm) along with two utility files.
Generating the Witness

### To run the witness file, use the following command:

```node circuit_js/generate_witness.js circuit_js/circuit.wasm input.json witness.wtns```

Here:

- circuit_js/generate_witness.js is the file that we want to run.
- circuit_js/circuit.wasm is the circuit wasm file we are feeding to the JavaScript file.
- input.json contains the input for the circuit.
- witness.wtns is the output file that contains the witness.

### Sample input.json file

```
{
    "x": 2,
    "y": 3
}
```

After running the witness generation command, you'll get a binary witness file. To convert it to JSON format, use:


npx snarkjs wtns export json witness.wtns witness.json

### Witness file (witness.json)
```
[
 "1",
 "47",
 "2",
 "3",
 "4",
 "12",
 "9"
]
```

The witness file is structured as follows: The first number is always one, representing constant operations within the circuit. This number can be ignored. Next comes the output, which, in this case, is only one value: 47.

To verify our understanding, let's quickly calculate the output based on the given inputs, where x = 2 and y = 3. The result is 47, which matches the correct output.

After the output, the witness file contains the inputs (2 and 3) followed by all the intermediate witness values. The number of witnesses in the file will always be the same as the number of constraints during the compilation, minus one. The reason for this may be related to the degree of freedom in a constraint system, although it's not entirely certain.