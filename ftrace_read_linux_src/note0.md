* `prepare_ftrace_return`
    * `leaq 8(%rbp), %rdi` `unsigned long *parent`
    * `movq 0x38(%rsp), %rsi` `unsigned long self_addr`
    * `movq (%rbp), %rdx` `unsigned long frame_pointer`
