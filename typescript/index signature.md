https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures

第一次见到比较奇怪的ts语法:
```
interface StringArray {
  [index: number]: string;
}
 
const myArray: StringArray = getStringArray();
const secondItem = myArray[1];
```

学习了一下，原来这个叫index signature
