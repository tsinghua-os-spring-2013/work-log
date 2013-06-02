* `prepare_ftrace_return`
    1. `unsigned long *parent`: 
        * `leaq 8(%rbp), %rdi`  
        * address of (return address)
    2. `unsigned long self_addr`: 
        * `movq 0x38(%rsp), %rsi` 
        * `subq $MCOUNT_INSN_SIZE, %rsi` 
        * previous %rbp value (?) - $MCOUNT_INSN_SIZE 
        * `#define MCOUNT_INSN_SIZE    5 /* sizeof mcount call */`
        * what's my `MCOUNT_INSN_SIZE`?
    3. `unsigned long frame_pointer`: 
        * `movq (%rbp), %rdx` 
        * previous %rbp value
