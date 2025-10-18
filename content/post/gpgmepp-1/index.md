+++
date = '2025-10-14T18:18:35+08:00'
draft = false
title = 'GpgME++ 库的使用（1）'
tags = ["GnuPG", "Library-usage", "Document-style"]
+++

## 0x00. 前言

这篇文章主要介绍了GpgME\+\+的使用，同志们应当了解什么是GnuPG，会用现代C++(C++20以上)进行编程哦！

## 0x01. 这是什么？

{{< quote author="GnuPG" source="GPGME" url="https://www.gnupg.org/software/gpgme/index.html">}}
GnuPG Made Easy (GPGME) 是一个旨在让应用程序更容易访问 GnuPG 的库。它提供了一个用于加密 、解密、签名、签名验证和密钥管理的加密 API。目前它使用 GnuPG 的 OpenPGP 后端作为默认选项，但 API 并不限于这个引擎。
{{< /quote >}}  


GpgME++(GPGMEPP)就是GPGME的C++绑定库。
可以发现，他的目标是“让应用程序更容易访问 GnuPG”，而不是“让应用程序更容易使用 OpenPGP”。
因此，这带来了一些特点，例如：
1. 它一离开 GnuPG 就是纸老虎。
2. 它（在默认情况下）会对 ~/.gnupg 做操作。
3. 它会调用 Pinentry 解密密钥。
4. 它在调用 OpenPGP Smartcard 上 遥遥领先 (
5. 接口相对友好，但保留有许多C-style(如使用const char*)

如果真的你打算使用的话...

## 0x20. 那...让我们开始吧!

### 0x21. 配置环境

假如你用Debian/Ubuntu，那应该用的是：
``` bash
sudo apt-get install libgpgmepp-dev
```
假如你用Fedora，那应该用的是：
``` bash
sudo dnf install gpgmepp-devel
``` 
同理，CentOS是:
``` bash
sudo yum install gpgmepp-devel 
```
假如你用Arch，那应该用的是：
``` bash
sudo pacman -S gpgmepp
```
{{< details summary="我把小众的发行版叠起来了" >}}
<detail>
我使用的是Gentoo，这是我的安装命令发生的变化:
``` bash
sudo emerge -a dev-cpp/gpgmepp:0
```
Opensuse应该是:
``` bash
sudo zypper install gpgmepp
```
Aosc好像最近挺有名的（GpgME++包括在GPGME中了，其他相似情况也是一样）:
``` bash
sudo oma install gpgme
```
Alpine（不要被gpgmepp包骗了）:
``` bash
sudo apk add gpgme-dev
```
Void Linux：
``` bash
sudo xbps-install -S gpgmepp-devel
```
致敬自由软件斗士Guix GNU/Linux：
``` bash
guix install gpgme
```
{{< /details >}}

### 0x22. Hello World!
``` C++
//helloworld.cpp
//使用前请保证知识有一个私钥哦！有多个的话可以改下面startKeyListing的参数
#include <gpgme++/context.h>
#include <gpgme++/global.h>
#include <gpgme++/data.h>
#include <gpgme++/keylistresult.h>
#include <gpgme++/signingresult.h>
#include <gpgme++/error.h>
#include <gpgme++/exception.h>
#include <memory>
#include <print>
#include <stdexcept>
#include <string_view>

std::string clearsign(std::unique_ptr<GpgME::Context>& context, std::string_view rawdata) {
    GpgME::Error error;
    context->startKeyListing("", true); // "" -> 匹配所有;多密钥可修改为UID/指纹;true -> 只列出私钥;
    GpgME::Key key;
    do {
        key = context->nextKey(error);
    } while ( !(error || key.canSign()) ); // 如果error为真 -> 无可用钥，跳出并报错;如果canSign为真 -> 此钥可用，跳出;
    context->endKeyListing();
    if (error) {
        throw GpgME::Exception{error}; //GpgME++指供了现成的Exception封装，可直接throw;
    };
    context->clearSigningKeys();
    error = context->addSigningKey(key); //Error可重用;
    if (error) {
        throw GpgME::Exception{error};
    };
    GpgME::Data inputdata{rawdata.data(), rawdata.size(), false}; //false -> inputdata相当于一个指针;
    GpgME::Data outputdata{};
    auto signingresult = context->sign(inputdata, outputdata, GpgME::Clearsigned);
    if (!signingresult.error().isSuccess()) { // xxResult最常用来当Error;当操作取消时，不视 为错误（error为false），但仍可throw;
        throw GpgME::Exception{signingresult.error()};
    };
    return outputdata.toString();
};

int main()
{
    GpgME::initializeLibrary();
    auto context = GpgME::Context::create(GpgME::OpenPGP);
    if (!context) {
        throw std::runtime_error("Unable to create context");
    };
    std::println("结果:\n{}", clearsign(context, "Hello World!"));
};
```
我们可以使用以下命令进行编译:
``` bash
g++ helloworld.cpp -o helloworld -std=c++23 `pkg-config --cflags --libs gpgmepp`
```
一切顺利的话，你就可以看到你的Hello World啦!
### WIP:0x23.基本对象的基本介绍

#### 1. Context

哇，多么好的对象啊(由衷赞赏)!Context的主要任务是完成与GnuPG的交互，所有我们要进行的操作都封装在这里。它是古希腊掌管

+ 加密，解密，签名，验证(见"0x23.基本操作&错误处理")
+ 密钥列举，密钥管理
+ 智能卡
+ 工作模式
+ 以及各种方法的异步版本

的神。  

**对象获取：**
1. `static std::unique_ptr<Context> GpgME::Context::create(Protocol proto);` 
2. `static Context *GpgME::Context::createForProtocol(Protocol proto);`
3. `std::unique_ptr<Context> createForEngine(Engine engine, Error *err = nullptr);`

Protocol可以取GpgME::OpenPGP和GpgME::CMS。
> 正常情况下，proto应当取GpgME::OpenPGP

> 如你所见，2)返回的是裸指针，所以强烈不建议使用哦!

**方法列表：**  
既然是核心类，肯定是要单开一部分的，所以，这里就不介绍了。  

#### 2. Data

Data对象是提供/获取数据的核心对象，被设计为缓冲区。没有Data就不能向基本操作传入数据。  

**对象获取：**  
> 这里可以使用构造函数啦！

##### 基于内存的数据缓冲区

> 既然是基于内存的，就不要指望用文件做参数时会自动回写啦！  
> 参数是用来读(入内存)的，不是用来写的哦(OwO)

1. `Data();`
2. `Data(const char *buffer, size_t size, bool copy = true);`
3. `explicit Data(const char *filename);`
4. `Data(const char *filename, off_t offset, size_t length);`
5. `Data(std::FILE *fp, off_t offset, size_t length);`

##### 基于文件的数据缓冲区

> 是的，这里传入文件句柄的构造函数才会自动回写

1. `explicit Data(std::FILE *fp);`
2. `explicit Data(int fd);`

##### 基于回调的数据缓冲区

1. `explicit Data(DataProvider *provider);`

> 众所周知，C++的流也是缓冲区，所以可以用DataProvider进行包装，有机会我会给出例子

**方法列表：** 

##### 通用操作

1. `void swap(Data &other);`
2. `bool isNull() const;`

##### 缓冲区操作

> 是很标准的缓冲区操作函数（不过读写指针在一起）

1. `ssize_t read(void *buffer, size_t length);`
2. `ssize_t write(const void *buffer, size_t length);`
3. `off_t seek(off_t offset, int whence);`
4. `Error rewind();`

> 如果需要多次传入同一数据，每次传入数据后都要用一次rewind();！

##### 文件操作(只是读文件！)

> 这里的设置只能读文件哦！

1. `char *fileName() const;`
2. `Error setFileName(const char *name);`
3. `Error setFileName(const std::string &name);`

##### 类型相关

1. `Encoding encoding() const;`
2. `Error setEncoding(Encoding encoding);`
3. `Type type() const;`
4. `std::vector<Key> toKeys(const Protocol proto = Protocol::OpenPGP) const;`

##### 其他操作

> `std::string toString();`超级常用哦

1. `std::string toString();`

2. `Error setFlag(const char *name, const char *value);`
3. `Error setSizeHint(uint64_t size);`

> 对于2,3，这里引用一下GpgME的文档

{{< quote author="The GnuPG Made Easy Reference Manual" source="6.3.2 Data Buffer Meta-Data" url="https://www.gnupg.org/documentation/manuals/gpgme/Data-Buffer-Meta_002dData.html">}}
起始版本：1.7.0
可通过此函数设置标志来控制数据对象的一些次要属性。属性通过以下 name 值进行标识：
size-hint（大小提示）
    value 是一个十进制数字，表示 gpgme 应为此数据对象假定的长度。当数据通过回调函数或文件描述符提供，但应用程序知道数据总大小时，此属性非常有用。如果设置此属性，OpenPGP 引擎可能会使用该值来决定缓冲区分配策略，并为进度信息提供总值。
io-buffer-size（I/O缓冲区大小）
    value 是一个十进制数字，表示用于内部 I/O 操作的内部缓冲区长度。该值上限为 1048576（1 MiB）。在某些环境下，较大的缓冲区可以提升基于回调的数据对象的性能，但具体效果很大程度上取决于运行环境和操作系统。此标志只能设置一次，并且必须在数据对象发生任何实际 I/O 操作之前设置。
sensitive（敏感数据）
    如果数值不为 0，则表示该数据对象包含敏感信息（如密码或密钥材料）。设置此标志后，gpgme_data_release 函数将以零值安全覆盖内部缓冲区。
此函数成功时返回 0。
{{< /quote >}}  

#### WIP:4.Key
这个东西就代表你的一个密钥，加密和签名都需要它。  
**方法列表：** 

#### WIP:5.Subkey

#### WIP:6.UserID

### WIP:0x24.基本操作

### WIP:0x25.错误处理
