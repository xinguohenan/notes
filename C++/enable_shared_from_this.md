std::enable_shared_from_this 有什么意义?

来自知乎的答案:

简单地说就是帮助你怎么在class内部拿到this的shared_ptr版本。
```
class A {
void func() {
  std::shared_ptr<A> local_sp_a(this);
  // do something with local_sp_a
}
}

int main() {
  A* a;
  std::shared_ptr<A> sp_a(a);
  a->func();
  // sp_a becomes dangling.
}
```

这样拿的话，表面上看起来你是拿到了一个this的shared_ptr版本，但是由于计数器和被管理的对象是分离的，因此相当于2个计数器（reference count都=1），一个被管理的对象。函数内部的那个计数器，在函数调用完成之后，认为reference count变成0了，于是释放掉了对应的object。main里构造的计数器当然不知道对应的object已经被释放了，于是就会boom。C++解决方案是通过继承一个类，这个类本质上会给被管理的object上加一个指向计数器的weak ptr，于是就可以正确地增加引用计数而不是搞出2个独立的计数器。

大家都知道

T * t = new T();

smart_ptr<T> t1 = t;

smart_ptr<T> t2 = t;

这是不对的，为什么要弄一个enable-balala 其实是一个道理

