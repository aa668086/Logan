# Logan

[![license](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat)](https://xxxxx/LICENSE)
[![Release Version](https://img.shields.io/badge/release-0.1.0-red.svg)](https://xxx/releases)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://xxx/pulls)
[![WeChat Approved](https://img.shields.io/badge/Platform-%20iOS%20%7C%20Android%20-brightgreen.svg)](https://xxx/wiki)

Logan is a lightweight case logging system based on mobile platform.

[中文说明](./README-zh.md)

![Logan](./img/logan_arch.png)

# Getting started

## Android

### Installation

Add the following content in the project `build.gradle` file:

```groovy
compile 'com.dianping.sdk:logan:x.x.x'
```

### Usage

You must init Logan before you use it. For example:

```java
LoganConfig config = new LoganConfig.Builder()
        .setCachePath(getApplicationContext().getFilesDir().getAbsolutePath())
        .setPath(getApplicationContext().getExternalFilesDir(null).getAbsolutePath()
                + File.separator + "logan_v1")
        .setEncryptKey16("0123456789012345".getBytes())
        .setEncryptIV16("0123456789012345".getBytes())
        .build();
Logan.init(config);
```

After you init Logan, you can use Logan to write a log. Like this:

```java
Logan.w("test logan", 1);
```

Logan.w method has two parameters:

- **String log**: What you want to write;
- **int type**: Log type. This is very important, best practices below content will show you how to using log type parameter.

If you want to write log to file immediately, you need to call flush function:

```java
Logan.f();
```

If you want to see all of the log file information, you need to call getAllFilesInfo function:

```java
Map<String, Long> map = Logan.getAllFilesInfo();
```

- key: Log file date;
- value: Log file size(Bytes).

#### Upload

Logan internal provides logging upload mechanism, in view of the need to upload the log to do the preprocessing. If you want to upload log file, you need to implement a SendLogRunnable:

```java
public class RealSendLogRunnable extends SendLogRunnable {

    @Override
    public void sendLog(File logFile) {
      // logFile: After the pretreatment is going to upload the log file
    }
}
```

Finally you need to call Logan.s method:

```java
Logan.s(date, mSendLogRunnable);
```

One of the first parameter is date array(yyyy-MM-dd).

## iOS

### Installation

Logan supports CocoaPods methods for installing the library in a project.

#### Podfile

Import Logan in Xcode project, add Logan in podfile.

```ruby
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.0'

target 'TargetName' do
pod 'Logan', '~> 0.1.0'
end
```

Then run the following command:

```bash
$ pod install
```

### Logan Init

You must init Logan before you use it:

```objc
NSData *keydata = [@"0123456789012345" dataUsingEncoding:NSUTF8StringEncoding]; 
NSData *ivdata = [@"0123456789012345" dataUsingEncoding:NSUTF8StringEncoding];
uint64_t file_max = 10 * 1024 * 1024;
// logan init，incoming 16-bit key，16-bit iv，largest written to the file size(byte)
llogInit(keydata, ivdata, file_max);

#if DEBUG
llogUseASL(YES);
#endif
```

### Usage

Write a log:

```objc
LLog(1, @"this is a test");
```

# Best Practices

Before Logan available, log report system is relatively scattered.

![Before_Logan](./img/before_logan.png)

To put it simply, the traditional idea is to piece together the problems that appear in the logs of each system, but the new idea is to aggregate and analyze all the logs generated by the user to find the scenes with problems.

The Logan core system consists of four modules:

- Input
- Storage
- BackEnd
- FrontEnd

![Logan_Process](./img/logan_process.png)

The new case analysis process is as follows:

![Logan_Case](./img/logan_case.png)

# Article

[A lightweight case logging system based on mobile platform developed by Meituan-Dianping — Logan](https://tech.meituan.com/Logan.html).

# Feature

In the future, we will provide a data platform based on Logan big data, including advanced functions such as machine learning, troubleshooting log solution, and big data feature analysis.

Finally, we hope to provide a more complete integrated case analysis ecosystem.

![Logan_System](./img/logan_system.png)

| Module | Open Source | Processing | Planning |
| :------: | :--: | :-----: | :-: |
| iOS  |   √  |        |    |
| Android | √ |  |  |
| Web |  | √ |  |
| Mini Programs |  | √ |  |
| Back End |  |  | √ |
| Front End |  |  | √ |

# Contributing

**For more information about contributing PRs and issues, see our [Contribution Guidelines](./CONTRIBUTING.md).**

# License

MIT
