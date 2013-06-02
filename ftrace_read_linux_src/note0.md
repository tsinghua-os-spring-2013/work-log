* `prepare_ftrace_return`
    1. `unsigned long *parent` 
        `leaq 8(%rbp), %rdi`  address of (return address)
    * `movq 0x38(%rsp), %rsi` `subq $MCOUNT_INSN_SIZE, %rsi``unsigned long self_addr` previous %rbp value? 
    * `movq (%rbp), %rdx` `unsigned long frame_pointer` previous %rbp value
