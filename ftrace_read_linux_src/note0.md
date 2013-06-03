* `prepare_ftrace_return`
    1. `unsigned long *parent`: 
        * `leaq 8(%rbp), %rdi`  
        * = address of (return address)
    2. `unsigned long self_addr`: 
        * `movq 0x38(%rsp), %rsi` 
        * `subq $MCOUNT_INSN_SIZE, %rsi` 
        * didn't push %rbp (so no %rbp on stack)
        * = return address (?) - $MCOUNT_INSN_SIZE (with respect to mcount)
        * `#define MCOUNT_INSN_SIZE    5 /* sizeof mcount call */`
        * `MCOUNT_INSN_SIZE` size of `callq` (?)
    3. `unsigned long frame_pointer`: 
        * `movq (%rbp), %rdx` 
        * %rbp is still the same %rbp in the prev frame (?)
        * = previous %rbp value (of the prev frame) (?) (with respect to mcount)
