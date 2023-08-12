1. 生成 YogaNode: https://github.com/facebook/yoga/blob/main/java/com/facebook/yoga/YogaNode.java
2. 基于 YogaNode 生成 ReactShadowNode，ReactShadowNode负责计算 layout 信息，将 layout 信息转换成平台 Native View 能理解的属性，投喂给真正的 NativeView。
   https://github.com/facebook/react-native/blob/main/packages/react-native/ReactAndroid/src/main/java/com/facebook/react/uimanager/ReactShadowNodeImpl.java
3. ViewManager 负责将 ShadowNode 转换成真正的 NativeView，用一个 map 做映射。
   https://github.com/facebook/react-native/blob/main/packages/react-native/ReactAndroid/src/main/java/com/facebook/react/shell/MainReactPackage.java

举例
1. ReactTextShadowNode 可以生成ReactTextUpdate，里面包含一些 layout 属性(padding/alignMode 信息等等).
https://github.com/facebook/react-native/blob/main/packages/react-native/ReactAndroid/src/main/java/com/facebook/react/views/text/ReactTextShadowNode.java
2. ReactTextViewManager 可以接收 ReactTextUpdate，并更新 ReactTextView（真正的 Native View）。
   https://github.com/facebook/react-native/blob/main/packages/react-native/ReactAndroid/src/main/java/com/facebook/react/views/text/ReactTextViewManager.java
