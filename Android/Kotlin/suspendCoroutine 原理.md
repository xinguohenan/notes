https://ljd1996.github.io/2021/05/19/Kotlin%E7%AC%94%E8%AE%B0%E4%B9%8B%E5%8D%8F%E7%A8%8B%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86/

suspendCoroutine
suspendCancellableCoroutine

当我们调用`Job.cancel()`时，suspendCancellableCoroutine会抛出CancellationException，suspend function 会用`try catch`之类的实现捕获异常，这样`Job`里具体执行的方法`block()`就不会继续执行了。
