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
