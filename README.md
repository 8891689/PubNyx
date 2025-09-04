# PubNyx

This is a high-performance public key (HASH160) cracking program aimed at brute force enumeration of public keys to find targets HASH160 Matching public key. It supports CPU and GPU Two operating modes to meet different performance requirements.(Hunt for Bitcoin public keys.)

# Main functions How It Works

1. Efficient Cracking: Highly optimized for HASH160, achieving extreme public key search speed.

2. CPU & GPU  Dual mode:

1. CPU  Mode: Utilizing multi-threaded parallel processing to fully leverage modern multi-core capabilities CPU  The performance.

2. GPU  Mode: Through CUDA  Offload large-scale parallel computing to NVIDIA GPU， Implementation ratio CPU  Faster cracking speed by several orders of magnitude.

3. Multi threading support (CPU): allows users to customize the use of CPU  Number of threads to balance system resources.

4. Flexible Starting Point: Supports searching from zero, specifying a public key, or a random point, facilitating collaboration or resuming broken downloads.

5. Instant status display: Real time display of the current cracking speed (MKeys/s), total number of checks, running time, and the range of public keys currently being checked in the terminal.

6. Result Output: Automatically saves the matching items (public key, public key HASH160) to the specified file after they are found.

7. For the puzzles, ```71``` or ```72``` and some more, there are no public keys are available, so if we can able to find public keys for those addresses, then [RCKangaroo](https://github.com/RetiredC/RCKangaroo) algorithm can be used to solve those puzzles.

8. ps Everyone knows that unspent BTC wallets don't share their public keys on the blockchain. We only know the address (decoded to get HASH160), so we need to crack the public key. Why is our cracking speed four times faster than others? Because SECP256K1 requires a lot of computation, which is the slowest and most expensive part of the cracking process. Here, we bypass it and replace it with more efficient algorithms, such as Baby-step Giant-step, Pollard's Rho, etc.

# Compilation
```
 make
```
Clean and rebuild
```
 make clean
```
# Features
```
./PubNyx -h
Usage: PubNyx [options]

Author: 8891689 (https://github.com/8891689)

A high-performance Public Key (HASH160) cracker with CPU and GPU support.

--- Target Options ---
  -a <hash>       Target a single HASH160 (required for GPU, optional for CPU).
--- Mode & Performance ---
  -gpu            Enable GPU cracking mode (much faster).
  -t <num>        (CPU) Number of CPU threads to use (default: 1).
  -R              Start search from a random point (for GPU, resets randomly every ~25165824 public key).
--- Other Options ---
  -o <file>       Output found matches to a file (default: found.txt).
  -bug            (GPU) Debug a specific key provided with -startkey.
  -startkey <hex> Start search from a specific point (CPU & GPU).
  -h, --help      Show this help message.
```
# Usage example

Test platform: Linux Debian based on Intel® Xeon® E5-2697 v4 2.30 GHz single-threaded environment using GPU NVIDIA GeForce GT 1030 2GB

1. Use GPU  Single pattern cracking HASH160 Specified increment

This is the most recommended and fastest way to use it.Assume you know the prefix of the X coordinate of the 70-digit puzzle public key.
```
./PubNyx -gpu -a 5db8cda53a6a002db10365967d7f85d19e171b10 -startkey 0290e6900a58d33393bc1097b5aed31f2e4e7cbd3e5466af958665bc0000000000
[+] Found matches will be written to: found.txt
[+] Mode: Sequential from specified starting point: 0290e6900a58d33393bc1097b5aed31f2e4e7cbd3e5466af958665bc0000000000
[+] GPU Cracking Mode Enabled.
[+] Using GPU: NVIDIA GeForce GT 1030
[+] Target Hash: 5db8cda53a6a002db10365967d7f85d19e171b10
[+] Kernel Config: 96 blocks, 256 threads/block
[+] Keys per batch: 25165824
[!] Public Key: 0290e6900a58d33393bc1097b5aed31f2e4e7cbd3e5466af958665bc0121248483, Hash: 5db8cda53a6a002db10365967d7f85d19e171b10
[+] Process finished in 24.73 seconds.
```
2. Use 1 CPU  Thread cracking If no starting point is specified, the default 020000000000000000000000000000000000000000000000000000000000000000
```
./PubNyx -t 1 -a 06eb89b4789e66286c7490fa1ee52d0b170699d0
[+] Found matches will be written to: found.txt
[+] Mode: Sequential from zero.
[+] CPU Public Key Cracking Mode Enabled.
[+] Starting with 1 CPU threads.
[!] Public Key: 020000000000000000000000000000000000000000000000000000000010123031, Hash: 06eb89b4789e66286c7490fa1ee52d0b170699d0
[+] CPU [51.2s] Total: 269627440    | Speed: 4.83    MKeys/s | Jumps: 0     | PubKey: 0200...000000000000000010123030    
[+] Process finished in 51.22 seconds.

```
3. Use GPU  Start searching from a random point and save the results to results.log
```
./PubNyx -gpu -R -a 5db8cda53a6a002db10365967d7f85d19e171b10 -o results.log
[+] Found matches will be written to: results.log
[+] Mode: Sequential from a random starting point.
[+] GPU Cracking Mode Enabled.
[+] Using GPU: NVIDIA GeForce GT 1030
[+] Target Hash: 5db8cda53a6a002db10365967d7f85d19e171b10
[+] Kernel Config: 96 blocks, 256 threads/block
[+] Keys per batch: 25165824
[+] GPU [23.4s] Total: 4605345792   | Speed: 206.47  MKeys/s | Jumps: 183   | PubKey: 0226...e9ea559d8c04f007cedddc9d ^C
```

# Get the public key
```
./rckangaroo -dp 16 -range 70 -start 200000000000000000 -pubkey 0290e6900a58d33393bc1097b5aed31f2e4e7cbd3e5466af958665bc0121248483
********************************************************************************
*                    RCKangaroo v3.0  (c) 2024 RetiredCoder                    *
********************************************************************************

This software is free and open-source: https://github.com/RetiredC
It demonstrates fast GPU implementation of SOTA Kangaroo method for solving ECDLP
Linux version
CUDA devices: 1, CUDA driver/runtime: 12.6/12.6
GPU 0: NVIDIA GeForce GT 1030, 1.94 GB, 3 CUs, cap 6.1, PCI 2, L2 size: 512 KB
Total GPUs for work: 1

MAIN MODE

Solving public key
X: 90E6900A58D33393BC1097B5AED31F2E4E7CBD3E5466AF958665BC0121248483
Y: D7319F127105F492FD15E009B103B4A83295722F28F07C95F9A5443EF8E77CE0
Offset: 0000000000000000000000000000000000000000000000200000000000000000

Solving point: Range 70 bits, DP 16, start...
SOTA method, estimated ops: 2^35.202, RAM for DPs: 0.210 GB. DP and GPU overheads not included!
Estimated DPs per kangaroo: 6.133.
GPU 0: allocated 300 MB, 98304 kangaroos. OldGpuMode: Yes
GPUs started...
Stopping work ...Keys/s, Err: 0, DPs: 626K/602K, Time: 0d:00h:19m/0d:00h:19m
Point solved, K: 1.207 (with DP and GPU overheads)


PRIVATE KEY: 0000000000000000000000000000000000000000000000349B84B6431A6C4EF1

```
# Thanks
```
gemini deepseek
```

# Sponsorship
If this project has been helpful or inspiring, please consider buying me a coffee. Your support is greatly appreciated. Thank you!
```
BTC: bc1qt3nh2e6gjsfkfacnkglt5uqghzvlrr6jahyj2k

ETH: 0xD6503e5994bF46052338a9286Bc43bC1c3811Fa1

DOGE: DTszb9cPALbG9ESNJMFJt4ECqWGRCgucky

TRX: TAHUmjyzg7B3Nndv264zWYUhQ9HUmX4Xu4
```
# ⚠️ # Disclaimer
1. Disclaimer: This tool is intended to help developers gain a deeper understanding of its workings and is for mutual learning and research purposes only.
2. Please understand the associated risks before using this tool. Decrypting someone else's private key is unethical and illegal. Please comply with local laws and regulations! Do not use this tool for any illegal purposes; you will be solely responsible for the consequences.
3. The developer is not responsible for any indirect or direct financial losses or legal liabilities resulting from the use of this tool.

