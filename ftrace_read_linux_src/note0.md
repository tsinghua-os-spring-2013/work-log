* `prepare_ftrace_return`
    * `leaq 8(%rbp), %rdi` `unsigned long *parent` return address
    * `movq 0x38(%rsp), %rsi` `unsigned long self_addr`
    * `movq (%rbp), %rdx` `unsigned long frame_pointer` previous %rbp value
