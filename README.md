# How to build an iOS JDK

First clone this repo with `--recurse-submodules` option.  

Then, you will need to apply `labs-openjdk/ios-jdk.patch` to `labs-openjdk-21` submodule.  
Setup a boot jdk (it is a JDK used when building the ios JDK). You can set it up by fetching one or simply download
GraalVM 23.1.3 and set your `JAVA_HOME` environment variable to it.  
To fetch a boot JDK execute:  
`mx -y --no-warning fetch-jdk --java-distribution labsjdk-ce-21 --to pathwhereyouwanttosetbootjdk --alias jdk21`.
Then in `labs-openjdk-21` submodule run configure command:  
```
./configure
    --with-conf-name=labsjdk
    --with-version-opt=jvmci-23.1.3-b33
    --with-version-pre=
    --with-vendor-name="GraalVM Community"
    --with-vendor-url=https://www.graalvm.org/
    --with-vendor-bug-url=https://github.com/oracle/graal/issues
    --with-vendor-vm-bug-url=https://github.com/oracle/graal/issues
```
Then run `make CONF_NAME=labsjdk graal-builder-image`.  
Once done, it should have generated all sources and headers needed to compile java static library for iOS.  
To compile java static library for iOS open `svm.openjdk.xcodeproj` and compile static library.  
When compiled you should have `libjava.a` ios static library in xcode build output folder.  

Then you will have to build SVM static library.  
To do so open `svm.graal.xcodeproj` and compile it for iOS.  
When done, you should have a `libjvm.a` static library in xcode project build output folder.  

You can then include both `libjava.a` and `libjvm.a` static libraries in your native app to enable java for your iOS app.
