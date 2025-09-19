#### Initial Information
- Encryption schemes aim to provide confidentiality but not integrity or authentication 
- Assume that the messages are bitstrings
- Cipher is defined over key space K, message space M and ciphertext space C. K = M = {${01}$}$^n$, C = {$01$}$^{2n}$
- Enc is often randomized, D is always deterministic
#### Desired Properties
- Correctness -> Decryption should always recover the original message 
- Efficiency -> E, d run in polynomial time in input size theoretically, practically run fast 
- Security -> Provides confidentiality 
#### One-time Pad
- Key Generation: The key is a randomly chosen bitstring -> both Alice and Bob know this key
- Use XOR for bitwise encryption of the plaintext M using the key K
- Ciphertext C is the encrypted bitstring that Alice sends to Bob over the insecure channel
- When Bob receives the ciphertext C, he XORs it with K to get M back
- Summary: 
	- KeyGen: Randomly generated new key
	- Enc(K,M) = $K \oplus M$
	- Dec(K,M) = $K \oplus C$ 
- If the key is used only once, then the system is IND-CPA
- If the key is reused for two or more different messages: 
	= $(K \oplus M_0) \oplus (K \oplus M_1)$
	= $M_0 \oplus M_1$
	-> Eve has deduced partial information 
- Problems: 
	- Key Generation: 
		- Key must be long, random and unique for every message
		- True randomness is costly and hard to scale
	- Key Distribution:
		- To send an n-bit message, Alice and Bob must first share an n-bit key
		- If you can securely share the key, you could have sent the message directly
		- Work-around: Pre-shared keys:
			- Use a secure channel to exchange many keys in advance
			- Use one-time pads over insecure channels (later when no secure channels)
	- Impractical to use in real life
#### Block Ciphers
- A cryptographic scheme consisting of encode/decode algorithms for a fixed-sized block of bits.
- Encryption implements a family of permutations on n bit strings -> one permutation for each K (a uniformly selected bit string of length k)
- $E_k(M)$ must be a permutation (bijective -> one-to-one and range covers its co-domain) on n-bit strings -> each input must correspond to exactly one unique output
	- Suppose $E_K(M)$ is not bijective -> 2 inputs might correspond to the same output so given the ciphertext, you can't uniquely decode the plaintext
	- Steps: 
		1. Inputs: k-bit key K and an n-bit plaintext M
		2. Output: an n-bit ciphertext C
		3. Written as {0,1}$^k$ x {0,1}$^n$ -> {0,1}$^n$
- $D_K (C)$ -> M: Decode
	1. Input a k-bit key and an n-bit ciphertext C
	2. Outputs an n-bit plaintext 
	3. Written as {0,1}$^k$ x {0,1}$^n$ -> {0,1}$^n$
	4. Inverse of encryption function
- Properties: 
	- Correctness: $E_K$ is a permutation, $D_K$ is its inverse
	- Efficiency: Encode/decode should be fast -> in microseconds, formally in polynomial time
		- Highly efficient on modern processors
		- Hardware Support: Many CPUs have built-in instructions for block ciphers 
	- Security: E behaves like a random permutation
- A raw block cipher is not IND-CPA secure (has deterministic outputs)  -> if Eve submits 2 messages and receives one of them back encrypted, she can ask to get $M_0$ encrypted, if it matches the ciphertext then its $M_0$ otherwise its $M_1$  
- Problems:
	- Deterministic
	- Fixed Block size:
#### Advanced Encryption Standard (AES)
- Developed 1997-2000 -> through NIST competition to select a new block cipher (key sizes: 128, 192 or 256 (most common))
- Block size: Always 128 bits
- Design: Substitution-Permutation Network
- AES only works on 128-bit blocks
	- Can't directly handle longer messages
	- Solution: Use modes of operation
	- Extend block ciphers to encrypt arbitrary-length messages and introduce randomness for security
#### Electronic Codebook (ECB) Mode
- To encrypt more than 128-bit messages, you can join multiple AES (with the same key)
- Enc(K, M) -> $C_1~||~C_2~||~C_M$ 
	- where M is split into m blocks of size n 
- Problem: AES-ECB is not IND-CPA secure (deterministic like AES)
#### CBC Mode (Cipher Block Chaining Mode)
- Solution to ECB's deterministic nature 
	- IV can't be reused otherwise this method also becomes deterministic
- Randomness is added by XORing the plaintext with the Initialization Vector (different for every encryption) for the first block.
- For the following blocks, the output of the block cipher is XORed with the plaintext. 
- $C_i = E_K(M_i \oplus C_{i - 1})$, $C_0 = IV$ (Encryption)
- $D_K(C_i) \oplus C_{i-1} = M_i$
- Enc(K, M): Split M in m plaintext blocks each of size n 
	- Choose a random IV
	- Compute and output $(IV, C_1,..., C_m)$ as the overall ciphertext
- Dec(K, C): Parse ciphertext as $(IV, C_1, ..., C_m)$ 
	- Decrypt each ciphertext and then XOR with the IV or previous ciphertext
- Problems: 
	- Encryption can't be parallelized as we need the previous block to finish before encrypting the next one. This is not a problem for decryption as it only requires the ciphertext as input.
- Padding: 
	- AES-CBC is only defined if the plaintext length is a multiple of the block size so the message needs to be padded until its a multiple of the block size. 
	- Failed Methods:
		- Pad with 0s -> Ambiguous if message ends with 0s
		- Pad with 1s -> Same problem
	- Correct Methods:
		- Bit padding: Append a 1, then fill with 0s
			- If message already fits, add a full block of padding
		- PKCS #7: Pad with the number of padding bytes
			- e.g. need 3 bytes, pad with 03 03 03
			- If no padding needed -> still add a full block of n bytes
#### CTR Mode
- This replicates a one-time pad, however, a nonce is used to induce randomness. The nonce is encrypted (acts like the pad) and XORed with the plaintext to produce the ciphertext. 
- There is a counter than increments each block to ensure the cipher output for each block is different. 
- Ciphertext is $(Nonce, C_1, ..., C_m)$
- To decrypt, the block cypher encryption is needed to encrypt the nonce again which is then XORed with the ciphertext to give the plaintext. 
- Plaintext is $(P_1, ..., P_m)$
- This method can be parallelized for encryption and decryption and it doesn't need padding as we can just cut off the parts of the XOR that are longer than the message.
- AES-CTR is IND-CPA secure assuming the nonce is randomly generated and not reused
#### Terminology (IV/Nonces)
- Definition: A one-use value that introduces randomness into encryption (random but public in many modes)
- In CTR mode, nonce ('number used once') - uniqueness matters more than randomness
- We treat IV/nonces interchangeably
- To get fresh IV/Nonces:
	- Random Generation: 
		- Use a secure random number generator to produce a new IV each time 
		- Works well for modes like CBC where randomness is enough 
	- Counters/sequence numbers:
		- Use a monotonically increasing counter (no reset/wrapping around)
		- Typical for CTR mode where uniqueness matters more
	- Combination:
		- Sometimes a random part + counter part is used (e.g. network protocols) to reduce the risk of collisions. 
- To ensure no reusing:
	- Implementation:
		- Never hard-code IVs
		- Don't reinitialize counters after reboot unless guaranteed no repetition
	- Storage if needed:
		- If long-lived systems, the the last used IV/counter to disk and reload after restart
	- Protocol Design: 
		- Some systems derive IVs deterministically from other unique values (e.g. message numbers, session IDs)
#### CFB Mode
- Also IND-CPA
- No padding messages, similar to CBC mode but worse if you reuse the IV
- Can't parallelize encryption