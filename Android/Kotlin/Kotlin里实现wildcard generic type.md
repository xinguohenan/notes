在Java中，我们可以用wildcard generic type:
```
public class TestApplication extends DaggerApplication {

    @Override
    public AndroidInjector<? extends TestApplication> applicationInjector() {
        return DaggerTestComponent.factory().create(this);
    }
    ...
}
```

AndroidInjector<? extends TestApplication> 这种就是wildcard generic type. Kotlin没有这样的语法，原因在这篇文章中做了解释:
https://kotlinlang.org/docs/generics.html#declaration-site-variance

那怎么办呢?
得用in, out等来修饰泛型。
```
Foo<out T : TUpper>
```
