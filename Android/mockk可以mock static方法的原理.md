mockk真不错，比Powermock好用很多。
mockk可以mock static方法，应该是基于JVMTI实现的，至少在Android上是:
https://github.com/mockk/mockk/tree/master/agent/android/src/main/jni/mockkjvmtiagent
