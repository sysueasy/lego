# Resume

## Octopus

核心职责：iOS 客户端研发。所负责产品包括：

* [Octopus 八達通](https://itunes.apple.com/hk/app/id1114430602)：为八達通卡客户提供服务的 APP，包括可以与卡余额互转的电子钱包功能，卡余额查询、交易查询、账单缴付、红包、票务等。
* [商用版八達通](https://itunes.apple.com/hk/app/id1306589192)：为商户提供服务的APP，主要功能包括二维码收款、实体卡收款、店铺管理、员工管理、交易报表等。

主要技术：

* 使用 Swift 编写，使用 Bridging-Header 与 Objective-C 代码同时编译。
* 在 iOS 端使用 Google J2ObjC 实现与 Java 代码同时编译，使用 Java 编写 SDK 并与 Android 端共用数据对象和部分业务逻辑。
* 使用蓝牙连接 Sony Felica 读写器的方法，实现个人用户和商户可以通过 iOS 设备进行八達通卡的交易（ iOS 11 只能读取 NDEF 数据）。
* 使用 Google Firebase 进行跨平台的消息推送、采集数据等。

我在项目中除了完成自己的职责，还会主动为项目组研究并引入新技术：

* 在 Swift 版本更新的过程中主动适配；在 iOS 10 Beta 时开始研究 Safe Area 和 iPhone X 的适配，并在发布后快速推出适配版本。
* 公司过去一直使用 SVN 做版本控制，我自己搭建了 GitLab 服务器，现在 iOS 和 Android 的同事们都已经转向 Git 。
* 为团队引入了 Slack 提高沟通效率，引入了 Trello 提高团队合作效率。
* 将项目中传统的 Core Data Stack 转为使用 NSPersistentContainer，并重写了一些会占用主线程资源的方法。
* 使用 Instrument 发现了内存泄漏并修复。

## PCCW Solutions

核心职责：iOS 客户端研发。所负责产品包括：

* [MyHKT](https://itunes.apple.com/hk/app/my-hkt/id664964247?mt=8)：为个人客户提供查阅账单、更新资料、查询数据流量等功能。
* [1010 Corporate](https://itunes.apple.com/hk/app/1o1o-corporate/id1194344336?mt=8)：为企业客户提供等功能。

除了 iOS 研发之外，我还接触了其他平台的研发：

* 在 [SmartCharge](https://itunes.apple.com/hk/app/smart-charge/id1191747509?mt=8) 项目中，使用了 Ionic 框架和 Angular 语言进行 Hybrid App 的开发并交付使用。为公司使用这种技术积累了经验，也对 Hybrid 和 Native 开发之间的优劣有了比较和认识。
* 在 [Tap & Go](https://itunes.apple.com/hk/app/tap-go-by-hkt/id1053808015?l=en&mt=8) 项目中，我在 Windows 平台上写了一个使用 USB 读卡器读取信息的小程序并交付使用，接触到了 C\# 语言、Visual Studio 和 Windows Forms Application 的开发。



