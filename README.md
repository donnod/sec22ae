# sec22ae

This repo contains the code and instructions for reproducing the evaluation results presented in the following paper:

[USENIX Security’22] *MAGE: Mutual Attestation for a Group of Enclaves without Trusted Third Parties* by Guoxing Chen and Yinqian Zhang

Install [linux-sgx-mage](https://github.com/donnod/linux-sgx-mage) before the evaluation:

### Efficiency of the measurement
- [MagePerformance](MagePerformance) contains the dummy enclave for evaluating the time (averaged from 10000 iterations) needed to derive one measurement with a ``.sgx_mage`` section containing only one page (by default). Evalutaion steps:
````
cd \path\to\MagePerformance
make; ./app
````
It prints something like:
````
Time taken for 10000 iterations of measurement derivation: xxxxx microseconds.
````

- To test with a ``.sgx_mage`` section of a different size (say 10000 pages), config linux-sgx-mage by editing [common/inc/sgx_mage.h](https://github.com/donnod/linux-sgx-mage/blob/master/common/inc/sgx_mage.h) (changing ``#define SGX_MAGE_SEC_SIZE 4096`` to ``#define SGX_MAGE_SEC_SIZE 40960000``) and re-install linux-sgx-mage. Instead of rebuilding the linux-sgx-mage from scratch, one short-cut is to remove the built ``libsgx_mage.a`` and ``sgx_mage.o`` files under the ``/linux-sgx-mage/sdk/mage`` folder, and follow the instructions to install linux-sgx-mage sdk. Evaluation steps:
````
cd \path\to\MagePerformance
make clean; make; ./app
````
It prints something like:
````
Time taken for 10000 iterations of measurement derivation: xxxxxxxxx microseconds.
````
### Memory overhead
- [MageMemory](MageMemory) contains the dummy enclave similar to the one in [MagePerformance](MagePerformance), except that the MAGE-related code and data are removed. The memory overhead can be calculated by comparing the sizes of built ``libenclave.so``s. For exmaple, with a ``.sgx_mage`` section containing only one page:
````
ll -lh /path/to/MagePerformance/libenclave.so 
-rw-r--r-- 1 root root 197K libenclave.so
 ll -lh /path/to/MageMemory/libenclave.so 
-rw-r--r-- 1 root root 139K libenclave.so
````

### Case Study: OPERA with MAGE
- Follow the instructions in [OPERA-MAGE: Open Remote Attestation for Intel's Secure Enclaves (MAGE version)](https://github.com/donnod/opera-mage).