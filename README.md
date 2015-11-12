OpenSGX: An open platform for Intel SGX
=======================================

Environments & Prerequisites
----------------------------
- Tested: Ubuntu 14.04-15.04, Arch
- Requisite: 
  - Ubuntu: apt-get build-dep qemu
  - Fedora: yum-builddep qemu

~~~~~{.sh}
Compile QEMU
$ cd qemu
$ ./configure-arch
$ make -j $(nproc)

Back to opensgx/
$ cd ..

Compile sgx library
$ make -C libsgx

Compile user-level code
$ make -C user
~~~~~

Run your first OpenSGX program
------------------------------

- Take user/demo/hello.c as an example.

~~~~~{.c}
#include <sgx-lib.h>

void enclave_main()
{
    char *hello = "hello sgx"\n";
    sgx_puts(hello);
    sgx_exit(NULL);
}
~~~~~

~~~~~{.sh}
$ ./opensgx -k
generate sign.key
$ ./opensgx -c user/demo/hello.c
generate hello.sgx
$ ./opensgx -s user/demo/hello.sgx --key sign.key
generate hello.conf
$ ./opensgx user/demo/hello.sgx user/demo/hello.conf
run the program
$ ./opensgx -i user/demo/hello.sgx user/demo/hello.conf
run the program with counting the number of executed guest instructions
~~~~~

Testing
-------

~~~~~{.sh}
$ cd user
$ ./test.sh test/simple
...
$ ./test.sh --help
[usage] ./test.sh [option]... [binary]
-a|--all  : test all cases
-h|--help : print help
--perf|--performance-measure : measure SGX emulator performance metrics
[test]
 test/exception-div-zero.c     :  An enclave test case for divide by zero exception.
 test/fault-enclave-access.c   :  An enclave test case for faulty enclave access.
 test/simple-aes.c             :  An enclave test case for simple encryption/decryption using openssl library.
 test/simple-attest.c          :  test network send
 test/simple.c                 :  The simplest enclave enter/exit.
 test/simple-func.c            :  The simplest function call inside the enclave.
 test/simple-getkey.c          :  hello world
 test/simple-global.c          :  The simplest enclave which accesses a global variable
 test/simple-hello.c           :  Hello world enclave program.
 test/simple-network.c         :  test network recv
 test/simple-openssl.c         :  test openssl api
 test/simple-quote.c           :  test network recv
 test/simple-recv.c            :  An enclave test case for sgx_recv.
 test/simple-send.c            :  An enclave test case for sgx_send.
 test/simple-sgxlib.c          :  An enclave test case for sgx library.
 test/simple-stack.c           :  The simplest enclave enter/exit with stack.
 test/stub.c                   :  An enclave test case for stub & trampoline interface.
 test/stub-malloc.c            :  An enclave test case for using heap
 test/stub-realloc.c           :  An enclave test case for sgx_realloc
~~~~~

Pointers
--------

- QEMU side
    - qemu/target-i386/helper.h    : Register sgx helper functions (sgx_encls, sgx_enclu, ...).
    - qemu/target-i386/cpu.h       : Add sgx-specific cpu registers (see refs-rev2 5.1.4).
    - qemu/target-i386/translate.c : Emulates enclave mode memory access semantics.
    - qemu/target-i386/sgx.h       : Define sgx and related data structures.
    - qemu/target-i386/sgx-dbg.h   : Define debugging function.
    - qemu/target-i386/sgx-utils.h : Define utils functions.
    - qemu/target-i386/sgx-perf.h  : Perforamce evaluation.
    - qemu/target-i386/sgx_helper.c: Implement sgx instructions.

- User side
    - user/sgx-kern.c         : Emulates kernel-level functions.
    - user/sgx-user.c         : Emulates user-level functions.
    - user/sgxLib.c           : Implements user-level API.
    - user/sgx-utils.c        : Implements utils functions.
    - user/sgx-signature.c    : Implements crypto related functions.
    - user/sgx-runtime.c      : sgx runtime.
    - user/sgx-test-runtime.c : sgx runtime for test cases.
    - user/include/ : Headers.
    - user/conf/    : Configuration files.
    - user/test/    : Test cases.
    - user/demo/    : Demo case.

Contribution
-------
We are more than happy to see any comments or feedback, as to improve
this project. To make contributions and take part in the project, there
are several ways you can do:
- Report bugs. Could either directly send us email or, even better, create an issue
  on github site. We will try our hard to take care of any report.
- Send patches. Could directly send us pull requests for minor changes. For larger
  changes, please contact us offline so we can discuss in more detail.

We specially appreciate those who actively make contributions to the project:
- Jon Gjengset
- Jethro Beekman
- Patrick Bridges

Contact
-------

Email: [OpenSGX team](sgx@cc.gatech.edu).

Authors
-------

- Prerit Jain <pjain43@gatech.edu>
- Soham Desai <sdesai1@gatech.edu>
- Seongmin Kim <dallas1004@gmail.com>
- Ming-Wei Shih <mingwei.shih@gatech.edu>
- JaeHyuk Lee <jhl9105@kaist.ac.kr>
- Changho Choi <zpzigi@kaist.ac.kr>
- Youjung Shin
- Taesoo Kim <taesoo@gatech.edu>
- Dongsu Han <dongsuh@ee.kaist.ac.kr>
- Brent Kang <brentkang@gmail.com>

NOTE. All authors at Gatech and KAIST equally contributed to the project

Publications
-------
- Paper on OpenSGX
```
OpenSGX: An Open Platform for SGX Research
Prerit Jain, Soham Desai, Seongmin Kim, Ming-Wei Shih, JaeHyuk Lee, Changho Choi, Youjung Shin, Taesoo Kim, Brent Byunghoon Kang, Dongsu Han
NDSS 2016


@inproceedings{opensgx,
        title        = {{OpenSGX: An Open Platform for SGX Research}},
        author       = {Prerit Jain and  Soham Desai and Seongmin Kim and  Ming-Wei Shih and  JaeHyuk Lee and  Changho Choi and Youjung Shin and Taesoo Kim and Brent Byunghoon Kang and Dongsu Han},
        booktitle    = {Proceedings of the Network and Distributed System Security Symposium},
        month        = feb,
        year         = 2016,
        address      = {San Diego, CA},
}
```

