最近遇到一个问题，C++写的library，在armv7a的版本上会crash。提示错误`SIGSEGV /SEGV_MAPERR`

排查了一下原因，发现是把指针从C++通过jni传给Java时，没有添加jlong的转换
Crash代码
```
std::shared_ptr<CppObj> cppObj = CreateCppObj();
env->CallVoidMethod(javaObj, javaMethod, cppObj.get());
```

修复代码
```
std::shared_ptr<CppObj> cppObj = CreateCppObj();
env->CallVoidMethod(javaObj, javaMethod, (jlong)cppObj.get());
```

这事儿也有点诡异。

不加jlong的转换，在Java层拿到的指针值(long类型)是一个很大的负数-7784369836185216460。
加上jlong的转换，在Java层拿到的指针值(long类型)就是一个正数2454086992。
