+++
date = '2025-10-08T18:08:04+08:00'
draft = true
title = '几个被忽略的 GPG 功能_简记'
+++

## 0x00.环境准备
出于方便，让我们定两个别名吧！
```
mkdir -p ~/dev/test/gpg1 ~/dev/test/gpg2
alias gpgt1='gpg --homedir=~/dev/test/gpg1'
alias gpgt2='gpg --homedir=~/dev/test/gpg2'
```
然后，分别生成key:
```
gpgt1 --quick-generate-key DFH1
gpgt2 --quick-generate-key DFH2

```

## 0x10.The Additional Decryption Subkey(ADSK)
ADSK是什么？简单来说，就是同时为多个加密子密钥加密（当然这里是\[R\]不是\[E\]）,但ADSK不一定是自己的。  
### 0x11.追加
为此，我们先为gpgt1导入DFH2  
```
gpgt2 --export DFH2 | gpgt1 --import
```
然后开始追加（先用colons看指纹）：  
```
gpgt1 --list-key --with-colons

pub:-:255:22:B5889F2B61D7AD49:1759928270:1854536270::-:::scESC:::::ed25519:::0:
fpr:::::::::6C97AF2B2F1B4181F1C162B8B5889F2B61D7AD49:
uid:-::::1759928270::E9DE857F2CFAE0072A66F7DEF963F162792C6B0D::DFH2::::::::::0:
sub:-:255:18:E20D7B492A0D2FB5:1759928270::::::e:::::cv25519::
fpr:::::::::A68650693D71F153BFEAD59EE20D7B492A0D2FB5:

```
这里是`A68650693D71F153BFEAD59EE20D7B492A0D2FB5`。  
```
gpgt1 --edit-key DFH1
...
gpg> addadsk
Enter the fingerprint of the additional decryption subkey: A68650693D71F153BFEAD59EE20D7B492A0D2FB5
```
我们就可以看到:
```
sub  cv25519/E20D7B492A0D2FB5
     created: 2025-10-08  expires: never       usage: R   
```
### 0x12.使用
让我们为DFH1加个密再用DFH2解密:
```
echo "Hello World!" | gpgt1 -a -e -r DFH1 | gpgt2 -d
```
可见:
```
gpg: encrypted with ECDH key, ID D37F8013B4C5C152
gpg: encrypted with cv25519 key, ID E20D7B492A0D2FB5, created 2025-10-08
      "DFH2"
Hello World!
```
如果用DFH1解密:
```
echo "Hello World!" | gpgt1 -a -e -r DFH1 | gpgt1 -d
gpg: encrypted with cv25519 key, ID D37F8013B4C5C152, created 2025-10-08
      "DFH1"
gpg: encrypted with cv25519 key, ID E20D7B492A0D2FB5, created 2025-10-08
      "DFH1"
Hello World!
```

## 0x20.TOFU

## 0x30.Revocation Key