---
titleTemplate: ':title | WriteUp - NewStar CTF 2024'
---
# 字里行间的秘密

```Plaintext
我横竖睡不着，仔细看了半夜，才从字缝里看出字来
```

打开附件，一个名为flag的word文件，一个key.txt，

word文件被加密，打开key文件，文字有明显的水印特征，放到vim就能看出存在零宽隐写，放到在线网站<https://www.mzy0.com/ctftools/zerowidth1/>默认参数拿到key：it_is_k3y

然后打开word发现没有flag，但CTRL+A发现第二行还是有内容的，将字体改为黑色就可发现flag