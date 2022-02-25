shared_ptr<void>采用了类型擦除，在创建shared_ptr的时候，额外传一个delete函数作为参数，这样就不需要类型也可以销毁相应的对象。
  https://stackoverflow.com/questions/5913396/why-do-stdshared-ptrvoid-work
