# 一加剪切板应用分析

## assets目录结构

```
address_ac.model:                 ASCII text
airport_short_name.model:         ASCII text
badword.txt:                      UTF-8 Unicode text, with very long lines, with no line terminators
BaiduNaviSDK_Resource_v1_0_0.png: Zip archive data, at least v2.0 to extract
BBB_test.txt:                     UTF-8 Unicode text
bubble_sms.in:                    UTF-8 Unicode text, with very long lines
channel:                          ASCII text, with no line terminators
city_ac.model:                    ASCII text
config.ini:                       ASCII text
dic.dat:                          Zip archive data, at least v2.0 to extract
html:                             directory
jcseg.properties:                 ASCII text
mapping.txt:                      UTF-8 Unicode text
md5.txt:                          ASCII text
obscure_word_ac.model:            ASCII text
obscure_word_explanation.dic:     data
parser.dic:                       data
parser.zip:                       Zip archive data, at least v2.0 to extract
pattern.zip:                      Zip archive data, at least v2.0 to extract
province_ac.model:                ASCII text
sharesmap.txt:                    UTF-8 Unicode text
shares.model:                     FORTRAN program, ASCII text
sms_address_parser.conf:          UTF-8 Unicode text, with very long lines
sms_bubble_stable_ac.index:       ASCII text
sms_bubble_stable_others.index:   ASCII text
sms_bubble_stable_regex.db:       SQLite 3.x database
sms_card_stable_ac.index:         ASCII text
sms_card_stable_others.index:     UTF-8 Unicode text
sms_card_stable_regex.db:         SQLite 3.x database
sms.cfg:                          ASCII text
sms_standard_parser.conf:         UTF-8 Unicode text
startword.txt:                    UTF-8 Unicode text
version_recorder.txt:             ASCII text, with no line terminators
w.dat:                            data
```

文本文件我们可以直接查看，数据库文件使用sqlite3或者其它工具查看也很方便，我们重点分析一下压缩和二进制文件的作用。

```
assets/BaiduNaviSDK_Resource_v1_0_0.png: Zip archive data, at least v2.0 to extract
assets/dic.dat:                          Zip archive data, at least v2.0 to extract
assets/obscure_word_explanation.dic:     data
assets/parser.dic:                       data
assets/parser.zip:                       Zip archive data, at least v2.0 to extract
assets/pattern.zip:                      Zip archive data, at least v2.0 to extract
assets/w.dat:                            data
```

### BaiduNaviSDK_Resource_v1_0_0.png文件分析

把此文件解压，得到下面的文件：

```
AndroidManifest.xml: Android binary XML
assets:              directory
res:                 directory
resources.arsc:      data
```

简单看了一下，应该是百度导航SDK，但是剪切板应用还需要这些东西吗？

### dic.dat文件分析

这个压缩文件文件的密码是`teddy@2016`，解压后发现下面的一些文件，前面的数字为文件总行数。其中`lex-main.lex`共169419行，里面当然也有不少敏感词。`lex-hotel.lex`是酒店的名称，我以前还真不知道有这么多种酒店呢！`lex-movie.lex`包含12064部电影名称，以后有需要了一定去参考一下。


```
      4 lex-ln-adorn.lex
      6 lex-en.lex
      7 lex-en-pun.lex
      8 lex-touris.lex
     10 lex-cemixed.lex
     13 lex-food.lex
     16 lex-org.lex
     21 lex-fname.lex
     21 lex-lang.lex
     28 lex-admin.lex
     28 lex-teddy-huawei.lex
     29 lex-net.lex
     67 lex-cn-title.lex
    101 lex-company.lex
    167 lex-ecmixed.lex
    169 lex-cn-mz.lex
    173 lex-units.lex
    187 lex-festival.lex
    191 lex-emoji.lex
    208 lex-sname.lex
    214 lex-dname-1.lex
    217 lex-dname-2.lex
    270 lex-nation.lex
    647 lex-lname.lex
   1269 lex-brand.lex
   1748 lex-stopword.lex
   2565 lex-cn-place.lex
   8271 lex-hot-words.lex
  12065 lex-movie.lex
  12641 lex-chars.lex
  20208 lex-hotel.lex
 169419 lex-main.lex
```

### obscure_word_explanation.dic与parser.dic分析

看了下代码，这两个文件貌似都需要看懂lib/armeabi/libtriedic.so中的一些函数才可以理解，等以后有能力再分析。

### parse.zip和pattern.zip分析

`parse.zip`解压后里面的txt文件大概都是正则表达式可能匹配的一些关键字，model文件目前尚未理解，怀疑跟txt文件中关键字的权重有关。

```
bankPatchConf.model:  UTF-8 Unicode text, with very long lines
blackAddressList.txt: UTF-8 Unicode text
blackNameList.model:  ASCII text, with CRLF, LF line terminators
blackNumberList.txt:  ASCII text
filmName.model:       ASCII text
flightAirport.model:  ASCII text, with CRLF, LF line terminators
flightCity.model:     ASCII text
name.txt:             UTF-8 Unicode text
operatorConf.model:   UTF-8 Unicode text, with very long lines
stations.model:       ASCII text, with CRLF, LF line terminators
whiteNameList.txt:    DOS executable (COM)
```

注：`whiteNameList.txt`大概被`file`错误识别了。

### w.dat分析

`w.dat`文件貌似没有在代码中直接使用，当然也可能相关字符串被加密混淆了，还有待研究。


## unknown目录结构

```
.
├── classes.jar
├── jcseg.properties
├── okhttp3
│   └── internal
│       └── publicsuffix
│           └── publicsuffixes.gz
└── themes
    ├── oneplus_black
    │   └── com.oneplus.clipboard.apk
    └── oneplus_white
        └── com.oneplus.clipboard.apk

6 directories, 5 files
```

### classes.jar分析

此文件解压后目录结构如下：


```
.
├── android
│   └── support
│       └── multidex
│           ├── BuildConfig.class
│           ├── MultiDexApplication.class
│           ├── MultiDex.class
│           ├── MultiDexExtractor$1.class
│           ├── MultiDexExtractor.class
│           ├── MultiDex$V14.class
│           ├── MultiDex$V19.class
│           ├── MultiDex$V4.class
│           ├── ZipUtil$CentralDirectory.class
│           └── ZipUtil.class
└── META-INF
    └── MANIFEST.MF
```

当然，里面还有一个`.readme`隐藏文件，内容如下：

```
This hidden file is there to ensure there is an res folder.
```

`android/support/multidex/`目录下的文件大概跟这个应用的主题修改有关系，尚未分析。

### com.oneplus.clipboard.apk分析

`apktool`反编译后的目录结构如下：

```
.
├── AndroidManifest.xml
├── apktool.yml
├── original
│   ├── AndroidManifest.xml
│   └── META-INF
│       ├── CERT.RSA
│       ├── CERT.SF
│       └── MANIFEST.MF
├── res
│   ├── drawable-xxhdpi-v4
│   │   ├── drawable_floatview_layout_background.png
│   │   └── drawable_floatview_layout_background_select.png
│   └── values
│       ├── colors.xml
│       ├── dimens.xml
│       ├── public.xml
│       └── styles.xml
└── smali
    └── com
        └── oneplus
            └── clipboard
                └── overlay
                    ├── BuildConfig.smali
                    ├── R$attr.smali
                    ├── R$color.smali
                    ├── R$dimen.smali
                    ├── R$drawable.smali
                    ├── R.smali
                    └── R$style.smali
```

看了一下xml文件和smali文件，貌似没有异常。


