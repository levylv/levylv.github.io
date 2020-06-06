---
title: java字符编码
date: 2019-06-11 18:13:30
categories: [编程开发,Java]
---

java中char的默认编码是UTF-16，2个字节，但实际操作系统中的默认编码一般都是UTF-8，所以在程序中如果没有指定操作系统的编码，一般都会进行UTF-16到UTF-8的编译码转换。当char的字节范围超过2个字节时，java会通过utf-16 pair的形式来解决。

```java
       String s = "ｮA";
        try {
            byte[] utf8 = s.getBytes("utf-8");//不指定的话就默认是OS的编码，一般UTF-8
            System.out.println(utf8.length);
//            System.out.println(s.getBytes("utf-16")[0]);
//            System.out.println(s.getBytes("utf-16")[1]);
//            System.out.println(s.getBytes("utf-16")[2]);
//            System.out.println(s.getBytes("utf-16")[3]);
            for (int i = 0; i < utf8.length; i++) {
                System.out.println(byteToBit(utf8[i]));
            }


            byte[] utf16 = s.getBytes("utf-16");
            System.out.println(utf16.length);
//            System.out.println(s.getBytes("utf-16")[0]);
//            System.out.println(s.getBytes("utf-16")[1]);
//            System.out.println(s.getBytes("utf-16")[2]);
//            System.out.println(s.getBytes("utf-16")[3]);
            for (int i = 0; i < utf16.length; i++) {
                System.out.println(byteToBit(utf16[i]));
            }

        }catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
```

结果：

```
4 //UTF-8的话，第一个字符3个字节，第二个字符1个字节(ACSII码)
11101111
10111101
10101110
01000001
6 //UTF-16的话，第一个字符4个字节，FEFF,FF6E pair，第二个字符2个字节(空字节+ASCII码)
11111110
11111111
11111111
01101110
00000000
01000001
```



对于byte和char：

byte就是1个字节，8位，int表示-128~127

char的话就是对应byte的编码表示，指定编码集后就对应char，一般不指定进行转换的话就是OS默认utf-8，char直接赋值的话存储的就是java默认utf-16