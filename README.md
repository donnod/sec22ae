# sec22ae

This repo contains the code and instructions for reproducing the evaluation results presented in the following paper:

[USENIX Security’22] *MAGE: Mutual Attestation for a Group of Enclaves without Trusted Third Parties* by Guoxing Chen and Yinqian Zhang

Install [linux-sgx-mage](https://github.com/donnod/linux-sgx-mage) before the evaluation:

### Efficiency of the measurement
- [MagePerformance](MagePerformance) contains the dummy enclave for evaluating the time (averaged from 10000 iterations) needed to derive one measurement with a .sgx_mage section containing only one page (by default).
- To test with a .sgx_mage section of a different size (say 10,000 pages), config [linux-sgx-mage](https://github.com/donnod/linux-sgx-mage/common/inc/sgx_mage.h) by changing ``#define SGX_MAGE_SEC_SIZE 4096`` to ``#define SGX_MAGE_SEC_SIZE 4096000`` and re-install linux-sgx-mage.

### Memory overhead
- [MageMemory](MageMemory) contains the dummy enclave similar to the one in [MagePerformance](MagePerformance), except that the MAGE-related code and data are
removed. The memory overhead can be calculated by comparing the sizes of built ``libenclave.so``s.