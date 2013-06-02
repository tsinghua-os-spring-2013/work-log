* `prepare_ftrace_return`
    * `leaq 8(%rbp), %rdi` `unsigned long *parent` address of (return address)
    * `movq 0x38(%rsp), %rsi` `unsigned long self_addr` previous %rbp value? 
    * `movq (%rbp), %rdx` `unsigned long frame_pointer` previous %rbp value
