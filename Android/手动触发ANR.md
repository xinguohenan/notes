测试发现三星手机上无法手动触发ANR，模拟器上可以。估计Pixel上应该也可以。
1. adb shell
2. run-as {your_debuggable_app_package_name}
3. kill -s SIGQUIT {debuggable_app_pid}
4. adb bugreport bugreport.zip
5. 解压bugreport.zip, 在FS/data/anr中，可以获取到anr的log。
