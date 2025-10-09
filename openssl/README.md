# OpenSSL benchmark

OpenSSL is an open-source security and privacy toolkit that executes SSL and TLS protocols.

System capabilites and limits for OpenSSL benchmark can be assessed with a benchmark using built-in "openssl speed" capabilities, where Server computation speed metrics for any cryptographic operation can be captured.

Resources related to OpenSSL application and its benchmarking procedure:

[OpenSSL project website](https://openssl-library.org/)

[openssl source repository](https://github.com/openssl/openssl)

[Feisty Duck](https://www.feistyduck.com/library/openssl-cookbook/online/openssl-command-line/performance.html)

[openssl-openbenchmarking](https://openbenchmarking.org/test/pts/openssl)

## Run OpenSSL benchmark using a Docker container

### 1. Ensure the docker is installed on the host
User should have the docker installed on their system.
Instructions for docker installation can be found under [Docker website](https://docs.docker.com/engine/install/)

### 2. Run openSSL benchmark using docker.

Benchmark an OpenSSL algorithm on a docker container :

```
  docker run -e core_count=<cpu-core-count> \
             -e algorithm=<OpenSSL-algorithm> \
             -e seconds=<time-in-seconds> \
             lnittala/benchmark-openssl-algorithm
```

OpenSSL benchmark test is containerized and published on docker hub registry at lnittala/benchmark-openssl-algorithm. This container builds the  OpenSSL tool of latest version and benchmarks a particular OpenSSL algorithm on multi-core for a given time.

`-evp` switch in OpenSSL speed tests supports AES-NI hardware acceleration on AMD EPYC based servers.
Note: It is important to always enable hardware acceleration for certain OpenSSL algorithm via `-evp` switch when testing to evaluate performance gains.

For instance, we can enable AES-NI hardware acceleration to benchmark AES-256-GCM algorithm of OpenSSL tool and achieve performance gain:

```
  docker run -e core_count=64 \
             -e algorithm="-evp aes-128-gcm" \
             -e seconds=15 \
             lnittala/benchmark-openssl-algorithm
```

##  Performance Metrics

Performance metrics from the OpenSSL algorithm benchmark can be tracked:

 S.No | Algorithm | Performance Metrics |
|---|---|---|
|1.| AES-256-GCM | Encryption Speed |
||   | Decryption Speed |
||   | Encryption/Decryption Latency |
|2.| AES-128-GCM | Encryption Speed |
||   | Decryption Speed |
||   | Encryption/Decryption Latency |
|3.| ChaCha20-Poly1305 | Encryption Speed |
||   | Decryption Speed |
||   | Authentication Speed |
||   | Latency to encrypt/decrypt a single operation |
|4.| ChaCha20 | Encryption Speed |
||   | Decryption Speed |
||   | Authentication Speed |
||   | Latency to encrypt/decrypt a single operation |
|5.| RSA4096 | Key Generation Time |
||   | Signing Operations Latency |
||   | Verification Operation Latency |
||   | Encryption Operations Latency |
||   | Decryption Operations Latency |
|6.| SHA512 | Hashing Throughput |
||   | Hash  Latency |
|7.| SHA256 | Hashing Throughput |
||   | Hash  Latency |

### Benchmark the popular OpenSSL algorithms
OpenSSL tool has various algorithms to cover different cryptographic operations. The most popular algorithms used as per the [OpenSource benchmark website](https://openbenchmarking.org/test/system/openssl) are as follows:

#### 1. AES-256-GCM
AES-256-GCM (Advanced Encryption Standard with 256-bit key in Galios/Counter Mode) is a widely used encryption algorithm that provides both confidentiality and authenticity for data.

[Algorithm Resource](https://www.geeksforgeeks.org/computer-networks/advanced-encryption-standard-aes/)

An AES-256-GCM benchmark is a performance test that measures how efficiently a server can encrypt and decrypt data using this cryptographic algorithm.

This benchmark measures the following metrics:

1. <u><b>Throughput</b></u>: This metric captures the <u>encryption speed</u> and <u>decryption speed</u>. Encryption speed captures the amount of data that can be encrypted per second; and decryption speed highlights the amount of data that can be decrypted per second. This metric is measured for various block sizes (16 bytes to 8KB+).

2. <u><b>Latency</u></b>: This metric measures the time to encrypt/decrypt a single operation; and is an influencing metric for real-time applications.

3. <u><b>CPU efficiency</u></b>: This can be measured in terms of CPU cycles per byte, CPU utilization or IPC (Instructions per cycle).


#### 2. AES-128-GCM

AES-128-GCM (Advanced Encryption Standard with 128-bit key in Galois/Counter Mode) is also an encryption algorithm that provides both confidentiality and authenticity for data, using a smaller key size than AES-256-GCM.

For this algorithm, we utilize the same performance metrics enlisted in AES-256-GCM algorithm.

#### 3. ChaCha20-Poly1305

ChaCha20-Poly1305 is a modern, high-performance authenticated encryption algorithm that combines the ChaCha20 stream cipher with the Poly1305 message authentication code (MAC).

We capture the following performance metrics:

1. <u><b>Throughput</b></u>: This metric captures the <u>encryption speed</u> and <u>decryption speed</u>; authentication speed. Authentication speed determines the MAC generation/verification rate.

2. <u><b>Latency</u></b>: This metric measures the time to encrypt/decrypt a single operation; and is an influencing metric for real-time applications.

3. <u><b>CPU efficiency</u></b>: This can be measured in terms of CPU cycles per byte, CPU utilization or IPC (Instructions per cycle).

#### 4. ChaCha20

ChaCha20 is a symmetric encryption algorithm that uses a 256-bit key for both encryption and decryption.The ChaCha20 encryption algorithm is designed to provide a combination of speed and security.

[Resource link](https://cyberw1ng.medium.com/understanding-chacha20-encryption-a-secure-and-fast-algorithm-for-data-protection-2023-a80c208c1401).

A ChaCha20 benchmark measures the encryption/decryption performance of the ChaCha20 stream cipher (without the Poly1305 authentication). This tests pure encryption speed.

We capture the following performance metrics:

1. <u><b>Throughput</b></u>: This metric captures the <u>encryption speed</u> and <u>decryption speed</u>; authentication speed.

2. <u><b>Latency</u></b>: This metric measures the time to encrypt/decrypt a single operation; and is an influencing metric for real-time applications.

3. <u><b>CPU efficiency</u></b>: This can be measured in terms of CPU cycles per byte, CPU utilization or IPC (Instructions per cycle).

#### 5. RSA4096

RSA-4096 is an asymmetric (public-key) encryption and digital signature algorithm using a 4096-bit key size. It's part of the RSA (Rivest-Shamir-Adleman) cryptosystem, one of the most widely used public-key algorithms.

An RSA-4096 benchmark measures the performance of RSA cryptographic operations using a 4096-bit key size. RSA is an asymmetric (public-key) encryption algorithm, so it has different performance characteristics than symmetric ciphers.

RSA Benchmarks measure:

1. <u><b>Key Generation time</b></u>: This metric measures the time taken to generate RSA key pair; which is computationally an expensive operation.

2. <u><b>Signing Operations</u></b>: This metric measures the time taken to create the digital signature and uses the private key . This operation involves modular exponentiation; which slows down the number of signing operations.

3. <u><b>Verification Operations</u></b>: This metric measures the time taken to verify the digital signature; and these operations use public key. These operations are fast, because this operation uses public exponent.

4. <u><b>Encryption Operations</u></b>: This metric measures the time taken to encrypt data with public key; and are fast due to the use of public exponent.

5. <u><b>Decryption Operations</u></b>: This metric measures the time taken to decrypt data with the private key; and are slow due to the use of modular exponentation.

#### 6. SHA512

SHA-512 (Secure Hash Algorithm 512) is a cryptographic hash function that produces a 512-bit (64-byte) hash value from input data of any size. It's part of the SHA-2 family designed by the NSA and published by NIST.

A SHA-512 benchmark measures the performance of the SHA-512 (Secure Hash Algorithm 512-bit) cryptographic hash function. It tests how quickly your system can compute SHA-512 hashes of data.

SHA Benchmarks measure:

1. <u><b>Hashing Throughput</b></u>: This metric measures the data hashed per second; this metric dould be tracked for the different data sizes.

2. <u><b>Latency</u></b>: This metric measures the time taken to hash a single message.

3. <u><b>CPU efficiency</u></b>: This can be measured in terms of CPU cycles per byte, CPU utilization or IPC (Instructions per cycle).

#### 7. SHA256

SHA-256 (Secure Hash Algorithm 256-bit) is a cryptographic hash function that produces a 256-bit (32-byte) hash value from input data of any size.

A SHA-256 benchmark measures the performance of the SHA-256 (Secure Hash Algorithm 256-bit) cryptographic hash function. It tests how quickly your system can compute SHA-256 hashes of data.

We use the same performance metrics enlisted in SHA512 algorithm.