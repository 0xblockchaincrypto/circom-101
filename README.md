## MiMC Hashing
Feistel cryptographic construction and its diffusion properties. Feistel is a cryptographic scheme that runs the input through multiple rounds of the same operation to obscure information. Each round of Feistel has two inputs and two outputs, which can be chained together to form a multiple operation. The goal is to securely encode or hash the information after going through several rounds.

The diagram for one round of Feistel is explained. It consists of a left-hand side (L) and a right-hand side (R). The right-hand side is fed into a function (F) along with a key (Ki) to produce an output (M). This output (M) is used to encrypt the left-hand side input. The left-hand side and M go through binary addition, resulting in the encrypted output from the left-hand side. Then, the encrypted output from the left-hand side and the right-hand side are exchanged to prepare for the next round.

An important point is that the right-hand side input in each round is not actually encrypted; it is only used as a reference. This is done to achieve diffusion, which prevents vulnerability to frequency attacks. In simple substitution ciphers like Caesar Cipher, a letter always gets encrypted to the same letter, making it vulnerable to frequency analysis. However, in Feistel, diffusion is achieved by encrypting the left-hand side with reference to the right-hand side, which also adds an element of randomness to further obscure the encryption.

The source of randomness in Feistel comes from the fact that the reference point (the right-hand side) determines the mask used for encryption. Since different right-hand side inputs will yield different masks, even if the left-hand side input remains the same, diffusion is achieved. To achieve a high level of encryption, Firestone uses multiple rounds (in this case, 20 rounds) to encrypt the input.

Additionally, the F function in Feistel is based on the Mimic-5 algorithm. It adds a constant (CI), generated beforehand, to the right-hand side input along with the key to construct the mask for encryption.

//

"sponge construction" and its application in Feistel MiMC (Mimic) encryption. The speaker explains that traditional hashing functions are "shotgun" functions, taking one input and producing one output. However, there are situations where multiple inputs need to be hashed together or multiple outputs are required. The sponge construction is a flexible solution for adapting hashing or encryption routines to handle multiple inputs and produce multiple outputs.

The construction consists of an encryption routine (F) that is repeatedly called, and it has two inputs and two outputs. This encryption routine F resembles the Feistel construction (Feistal), where each round of the encryption takes two inputs and produces two outputs.

The state of the encryption is divided into two parts: the "active" part (R bits) that takes inputs and produces outputs and the "capacity" part (C bits) that doesn't directly affect the outputs. This separation is necessary to achieve effective diffusion, providing an independent source of randomness for the actively encoded portion.

The encryption function F with its two inputs and two outputs perfectly fits the Feistel construction (five-store), making it ideal for integration into the sponge construction. In this case, MiMC 5 Sponge, where the F function is MiMC 5 Feistel, is used.

The sponge construction operates in two phases: the "absorbing" phase and the "output" phase. In the absorbing phase, each input is added to the R portion of the encryption state, and the encryption routine is repeatedly run to incorporate new information. This creates a repetitive and rhythmic absorbing process.

In the output phase, the hashing output is churned out from the R bits of the encryption state. Then, the state is encrypted again, producing additional outputs. This process allows the sponge construction to handle multiple inputs and produce multiple outputs with ease.

The sponge construction's adaptability and ability to handle arbitrary numbers of inputs and produce outputs of arbitrary length make it a powerful tool in cryptographic applications. It offers a solution for scenarios where traditional one-to-one hashing functions are not sufficient.

## Technical Details
`MiMCSponge` contains the circom code for the circuit

## Steps to run
generate the `input.json` file.

compile the code `npx circom2 MiMC5Sponge.circom --wasm`

run the program `node MiMC5Sponge_js/generate_witness.js MiMC5Sponge_js/MiMC5Sponge.wasm input.json output.wtns`

Translate the code from `wtns` to `json` `npx snarkjs wtns export json output.wtns output.json`

## Using Solidity
You can deploy the code and check with the same input. It should give same hash output

**This repo does not contains the code of `pedersen hash` function**