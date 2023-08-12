1. 生成 YogaNode: https://github.com/facebook/yoga/blob/main/java/com/facebook/yoga/YogaNode.java
2. 基于 YogaNode 生成 ReactShadowNode，ReactShadowNode负责计算 layout 信息，将 layout 信息转换成平台 Native View 能理解的属性，投喂给真正的 NativeView。
   https://github.com/facebook/react-native/blob/main/packages/react-native/ReactAndroid/src/main/java/com/facebook/react/uimanager/ReactShadowNodeImpl.java
3. ViewManager 负责将 ShadowNode 转换成真正的 NativeView，用一个 map 做映射。
   https://github.com/facebook/react-native/blob/main/packages/react-native/ReactAndroid/src/main/java/com/facebook/react/shell/MainReactPackage.java
