最近遇到一些问题，我们在Android上遇到native crash后，用release so生成symbol时，minidump生成的call stack是错的。

猜测原因可能是，我们用的so是用g0进行build的，就是不带任何符号信息。
这样的好处是so size会更小，但是因为不带任何符号信息，所以breakpad在分析call stack时，只能根据function的地址范围进行搜索，可能会生成错的call stack。

解决办法就是，要么release so别用g0，用默认值(g2)。
或者就是生成release so对应的带符号的so，再进行分析。

-glevel
-ggdblevel
-gvmslevel
Request debugging information and also use level to specify how much information. The default level is 2.

Level 0 produces no debug information at all. Thus, -g0 negates -g.

Level 1 produces minimal information, enough for making backtraces in parts of the program that you don’t plan to debug. This includes descriptions of functions and external variables, and line number tables, but no information about local variables.

Level 3 includes extra information, such as all the macro definitions present in the program. Some debuggers support macro expansion when you use -g3.

If you use multiple -g options, with or without level numbers, the last such option is the one that is effective.

-gdwarf does not accept a concatenated debug level, to avoid confusion with -gdwarf-level. Instead use an additional -glevel option to change the debug level for DWARF.

https://gcc.gnu.org/onlinedocs/gcc/Debugging-Options.html

对于release so， breakpad在分析时可能产生错误的call stack。StackOverflow上有一个类似的解释:

https://stackoverflow.com/questions/46804270/objdump-disassemble-doesnt-match-source-code

GOMP_ordered_end@@GOMP_1.0+0x70，这个只是说调用了一个内部方法(内部方法因为并不需要export, 符号信息被删除的一干二净)，这个内部方法代码位置在GOMP_ordered_end方法后偏移112个字节而已。并不是说调用了GOMP_ordered_end()方法。
```
Don't get distracted by GOMP_ordered_end@@GOMP_1.0+0x70 mark in assembly code. All it says is that this calls some local library function (for which objdump didn't find any symbol info) which happens to be located 112 bytes after GOMP_ordered_end. This is most likely gomp_resolve_num_threads
```
