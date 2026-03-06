# EpMedia-16kb
原项目为[EpMedia](https://github.com/yangjie10930/EpMedia)，这里进行了二次编译适配了android 16kb

## 使用说明
下载项目后file路径下的so可以直接使用，里面是编译好的16kb so库

## 编译说明
prebuilt目录下你需要编译什么样的so，就在file下把需要依赖的so替换，并在下面填入自己想编译的架构
```
  ndk {
            abiFilters  "armeabi-v7a"
     }
```

CMakeLists.txt下面ffmpeg-4.2.2的源码路径设置为自己的。这里给一个下载地址[ffmpeg4.2.2](https://github.com/srcstudy/ffmpeg)
