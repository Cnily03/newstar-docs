---
titleTemplate: ':title | WriteUp - NewStar CTF 2024'
---
# 遗失的拉链

拉链的英文是zip，这里也是考的www.zip泄露

使用如dirsearch之类的工具，可以帮我们快速完成源码泄露的检查

![使用dirsearch进行检查](/assets/images/wp/2024/week2/yishidelalian_1.png)

可以看到存在www.zip 泄露，访问后下载、解压得到源代码

pizwww.php内容如下：

```php
<?php
error_reporting(0);
//for fun
if(isset($_GET['new'])&&isset($_POST['star'])){
    if(sha1($_GET['new'])===md5($_POST['star'])&&$_GET['new']!==$_POST['star']){
        //欸 为啥sha1和md5相等呢
        $cmd = $_POST['cmd'];
        if (preg_match("/cat|flag/i", $cmd)) {
            die("u can not do this ");
        }
        echo eval($cmd);
    }else{
        echo "Wrong";

    } 
}
```

php中使用这些函数处理数组的时候会报错返回 `false` 从而完成绕过

命令执行过滤了`cat`，使用`tac`代替。`flag`被过滤，使用`fla*`通配符绕过

![hackbar传参](/assets/images/wp/2024/week2/yishidelalian_2.png)

这里简单的传递参数 使用hackbar就行

或者是这样`cmd=echo file_get_contents("/fla"."g");`