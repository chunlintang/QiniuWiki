##### 1，引用变量

> 概念

引用变量就是使用不同的名字访问同一个变量的内容，使用&符号。

> 工作原理

&符号

unset只会取消引用，不会销毁内存空间。
```php
<?php

$a = 1;
$b = &$a;
unset($b);
echo $a . "\n"; // 1
```

写出如下程序的输出结果
```php
<?php
$data = ['a', 'b', 'c'];
foreach($data as $key => $value) {
    $val = &$data[$value];
}
```
运行程序时，每一次循环结束后变量$data的值是什么？

['a', 'b', 'c']/['b', 'b', 'c']/['b', 'c', 'c']

程序结束后，$data的值是什么？

['b', 'c', 'c']

##### 2，数据类型

php中字符串有哪三种定义方式以及各自定义的区别?

> 数据类型

字符串定义方式三种：单引号、双引号、heredoc和newdoc

区别:

- 单引号不能解析变量
- 单引号不能解析转义字符，只能解析单引号和反斜线本身
- 变量和变量，变量和字符串，字符串和字符串之间使用.连接
- 双引号可以解析变量，变量可以使用特殊字符和{}包含
- 双引号可以解析所有的转义字符，也可以使用.来连接
- 单引号效率高于双引号

```php
$sql = "SELECT * FROM users WHERE name = '$name'";
$sql = 'SELECT * FROM users WHERE name = \''.$name.'\''; // 效率高
```

heredoc类似于双引号，newdoc类似于单引号，两者都用来处理大文本

> hreadoc
```php
<?php

$str = <<< Eot
............
EoT
```

> newdoc
```php
<?php

$str = <<< 'EoT'
.............
EoT
```

> 延伸考点

- 数据类型

三大数据类型：标量、复合、特殊

1） 浮点类型不能用于比较运算符中

浮点类型不能用于比较精确相等的运算，有精度丢失，比如:

```php
$a = 0.1;
$b = 0.7;

$a + $b != 0.8;

$a + $b = 0.7999;
```

2）布尔类型

false的七种情况:
```
0, 0.0,  ' ', '0', false, array(), NULL
```

3） 数组类型

- 超全局数组

```
$_GET, $_POST, $_GLOBALS, $_EQUEST, $_SESSION, $_COOKIE, $_SERVER, $_FILES, $_ENV

$_SERVER['SERVER_ADDR']; //服务器端ip
$_SERVER['SERVER_NAME']; //服务器名称
$_SERVER['REQUEST_TIME']; //请求时间
$_SERVER['QUERY_STRING']; // ?id=123 问号后面的参数串
$_SERVER['HTTP_REFERER']; //上级请求页面
$_SERVER['HTTP_USER_AGENT']; //头信息
$_SERVER['REMOTE_ADDR']; //客户端ip地址
$_SERVER['REQUEST_URI']; // /api/user/info
$_SERVER['PATH_INFO']; // 处理路由 http://www.baidu.com/user.php/user/reg?status=ghost user/reg就是path_info
```

4）NULL

三种情况：直接赋值为NULL、未定义的变量、unset销毁的变量

##### 3，常量

常量的定义有两种方式：const和define

- define是函数const不是
- const更快，是语言结构，define是函数
- define不能定一类的常量，const可以

> 预定义常量

```php
__FILE__ // 所在文件的文件路径+文件名
__LINE__ // 所在行号 
__DIR__ // 所在目录 
__FUNCTION__ // 所在函数体函数名称 
__CLASS__ // 类名 
__TRAIT__ // trait名称 
__METHOD__ // 类名+方法名
__NAMESPACE__ // 命名空间
```

##### 4，运算符

- 实例:foo()和@foo()之间的区别

> php中运算符的错误控制符@的使用，作用。PHP支持一个错误控制符@，当其放在一个php表达式之前，该表达式可能产生的所有错误信息都被忽略掉。

- 等值判断（false的七种情况都相等）

```
0.0 == 0 // true
' ' == false // true
```

- 递增递减运算符不影响布尔值
- 递减NULL值没有效果
- 递增NULL值为1
- 递增递减运算符在前就先运算符后返回，在后就先返回后运算
- ||和&&与or和and的优先级不同，前者优先级高
- and/or优先级最低

```php
$a = false || true; // $a = true
$b = false or true; // true

$a = 0;
$b = 0

// 好大的坑
if ($a = 3 > 0 || $b = 3 > 0) {
    $a++;
    $b++;
    echo $a . "\n";
    echo $b . "\n";
}

优先级：> || =

先判断 3 > 0 true 此时短路($a = true, $b = 0),进入if,$a = true,true++还是true,最后输出1,1
```

##### 5，流程控制考察点

1）php遍历数组的三种方式以及各自的区别。延伸：分支结构

> for、foreach、while,list(),each()结合。

- for循环只能遍历索引数组；
- foreach可以遍历索引数组和关联数组；
- 联合使用while,list(),each()同样可以遍历索引和联合数组；
- while,list(),each()不会对数组reset(),不会对数组指针进行操作；
- foreach遍历数组会对数组进行reset操作；

2）分支结构

```php
if.....elseif.....

switch($var) {
    case exp1:
    ....
    break;
    case exp2:
    ....
    break;
    default:
    .....
}
```

elseif基本原则，总把优先范围小的放在前面处理；switch后面的控制表达式($var)只能是整型、浮点型、字符串。continue语句作用到switch类似于break;

注意点：

```php
for() {
    switch($val) {
        case exp1:
        .....
        continue2;
        case exp2:
        .....
        break;
        default:
        .....
    }
}
```

> 如果要跳出switch外的循环，要使用continue2；switch.....case会生成调转表（索引表），直接跳转到对应的case，不会一层一层判断，效率高。如果条件比一个简单的比较要复杂得多，或者在一个很多次的循环中，那么switch语句可能会快一些。

(1)，如何优化多个if.....elseif...语句的情况？

将可能性较大的尽量往前放；根据表达式的类型（如果表达式的类型是整型、浮点型或者字符串）可以使用switch....case...语句。

##### 6，自定义函数及内部函数

```php
<?php
$count = 5;

function get_count()
{
    static $count; // NULL
    return $count++;
}

echo $count . "\n"; // 5
++$count;

echo get_count() . "\n"; // NULL 没有输出,第一次调用没有初始化
echo get_count() . "\n"; // 1
```

变量的作用域和静态变量

延伸：
- 函数的参数以及参数的引用传递；
- 函数的返回值以及引用返回；
- 外部文件的导入；
- 系统内置函数

函数外部的变量不能直接在函数内部调用，函数外部的变量是全局变量，内部的是局部变量，要想使用全局变量，使用global关键字；

```php
$outter = 'str';

function myfunc()
{
    global $outter;
    echo $outter;
}
```

另外也可以使用$GLOBALS['outter']等超全局函数。

静态变量仅在局部函数域中存在，但当程序执行离开此作用域时，其值并不会消失；

> static关键字

- 仅初始化一次
- 初始化时需要赋值
- 每次执行函数该值会保留
- static修饰的变量是局部的，仅在函数内部有效
- 可以记录函数的调用次数，从而可以在某些条件下终止递归

> 默认情况下，函数参数是通过值传递的，如果允许函数修改参数的值，必须通过引用传递参数.

```php
<?php
$a = 1;

function myfunc($a)
{
    $a = 2;
}

echo $a; // 1 默认是通过值传递的

function myfunc(&$a)
{
    $a = 2;
}

echo $a; // 2
```

函数的返回使用return,如果没有return默认返回的是null。

> 函数的引用返回：从函数返回一个引用，必须在函数声明和指派返回值给一个变量时都使用引用运算符&

```php
function &myfunc()
{
    static $b = 10;
    return $b;
}

echo myfunc();  // 10
$a =  &myfunc();

$a = 100;

echo myFunc(); // 100
```

###### 外部文件的导入：
include/require语句包含并运行指定文件;如果给出文件路径名称，则从include_path中查找

- 文件记载过程中未找到文件include会发出一条警告，这一点和require不同，后者会爆出一个致命错误；
- require(incliude)/require_once(include_once)唯一区别是php会检查该文件是否被包含过，如果是则不会再次包含；

###### 系统内置函数
- 时间日期函数：```date()/strtotime()/mktime()/time()/microtime()/date_default_timezone_set()```
- ip处理函数：```ip2long()/long2ip()```
- 打印处理：```print()/pringf()/print_r()/echo/sprintf()/var_dump()/var_export()```
- 字符串处理函数：```implode()/explode()/join()/strrev()/trim()/ltrim()/rtrim()/strstr()/mumber_format()/...(strstr(str1, str2, boolean))//返回str2在str1中第一次出现的次数，并返回```
- 数组处理函数：```array_keys()/array_values()/array_shift()/array_unshift()/array_diff(差集)/array_intersect(交集)/array_merge()/...```

典型题目：

```php
<?php
$var1 = 5;
$var2 = 10;

function foo(&$my_var)
{
    global $var1;
    $var1 += 2;
    $var2 = 4;
    $my_var += 3;
    return $var2;
}

$my_var = 5;
echo foo($my_var) . "\n"; // 4
echo $my_var . "\n"; // 8
echo $var1 . "\n"; // 7
echo $var2 . "\n"; // 10
$bar = 'foo';
$my_var = 10;
echo $bar($my_var) . "\n"; // 4
```

##### 7，正则表达式
> 正则表达式的作用：分割、查找、匹配、替换字符串

- 分隔符：```正斜线(/)、hash符号(#)、取反符号(~)```
- 通用原子：```\d、\D、\w、\W、\s、\S```
- 元字符：```. * ? ^ $ + {n} {n,} {n,m} [] () [^] | [-]```

验证手机号码、邮箱等。
```php
// 获取字符串
// 1，反向引用
$str = '<b>abc</b>';
$pattern = '/<b>.*<\/b>/';
$str = preg_replace($pattern, '\\1', $str);
// 2，贪婪模式(.*?取消贪婪/)
$str = '<b>abc</b><b>bcd</b>';
$pattern = '/<b>(.*?)<\/b>/';
//或者
$pattern = '/<b>(.*)<\/b>/U';
$str = preg_replace($pattern, '\\1', $str);
// 分割
$pattern = '/<p>(.*?)<\/p>/'; 
$string = '这个字符串中的<p>php</p>和<p>mysql</p>被分割了！'; 
$result = preg_split($pattern, $string, -1, PREG_SPLIT_DELIM_CAPTURE); 

// 中文匹配 u当成16进制处理 + utf8中文范围```0x4e00-0x9fa5```,gb2312范围0xb0-0xf7,0xa1-0xfe
$str = "中文";
$pattern = '/[\x{4e00}-\x{9fa5}]+/u';
// 如果是gb2312编码的
//$pattern = '/[' . chr(0xb0) . '-' . chr(0xf7) . '][' . chr(0xa1) . '-' . chr(0xfe) . ']/';
preg_match($pattern, $str, $match);
print_r($match);


// 验证一个139开头的手机号码(\d数字)
$str = '13988888888';
$pattern = '/^139\d{8}$/';
preg_match($pattern, $str, $match);
print_r($match);

// 取出页面中img的src值
$str = '<img alt="高清无码" id="img-1" src="http://img.baidu.com/s1/av.jpg" />';
$pattern = '/<img.*?src="(.*?)".*?\/?>/i';
preg_match($pattern, $str, $match);
print_r($match);
```

> 正则表达式PCRE函数：```preg_replace()/preg_match()/preg_match_all()/preg_split()```

- preg_match : 正则匹配
- preg_match_all : 匹配所有
- preg_replace : 正则匹配替换
- preg_split : 正则表达式分隔字符串

##### 8，文件处理

> 文件读取和写入 
- fopen函数，打开一个文件，打开时需要指定打开模式;
- 打开模式：
- ```r/r+``` 只读方式打开文件指针指向开头/读写方式打开文件指针指向开头 
- ```w/w+``` 只写方式打开文件指针指向开头并且清空文件/读写方式打开文件指针指向开头，清空文件，w模式如果文件不存在会创建
- ```a/a+```追加的写入方式文件指针指向文件末尾/追加的读写入方式文件指针指向文件末尾,文件不存在会创建
- ```x/x+```以写入的方式创建打开文件，如果文件已存在会返回错误false，不存在才会创建/x+ 创建并以读写方式打开

- fwrite写入
- fclose关闭文件

不需要打开文件的函数```file_get_contents```和```file_put_contents```

file读取写入数组中,readfile读取文件写入缓冲区

访问远程文件开启```allow_url_fopen```,http协议连接只能使用只读，ftp协议可以使用只读或者读写

> 目录操作

- 名称相关 : ```basename()/dirname()/pathinfo()```
- 目录读取 : ```opendir()/readdir()/closedir()/rewinddir()```
- 目录删除 : ```rmdir()```文件为空才能删
- 目录创建 : ```mkdir()```
> 其他函数

- 文件大小 : ```filesize()```
- 目录大小 : ```disk_free_space()``` 磁盘剩余空间/ ```disk_total_space()``` 磁盘总大小
- 文件拷贝 : ``copy()```
- 删除文件 : ```unlink()```
- 文件类型 : ```filetype()```
- 重命名 : ```rename()```
- 文件属性 : ```file_exists()```/```is_readable()```/```is_writable()```/```is_executable()```是否可执行/```filectime()``` 创建时间/```fileatime()```访问时间/```filemtime()```整个创建时间
- 文件锁 : ```flock()```
- 文件指针 : ```fseed()/ftell()/rewind()```

eg:

不断在文件hello.txt头部写入一行"Hello World"字符串?

```php
<?php

$file = './hello.txt';

$handle = fopen($file, 'r'); // 这里如果用r+或把原本内容覆盖掉
$content = fread($handle, filesize($file);
$content = 'Hello World' . $content;
fclose($handle);
$handle = fopen($file, 'w');
fwrite($handle, $content);
fclose($handle);
```

通过php函数的方式遍历目录？

```php
<?php

$dir = '../../360face';

function loopDir($dir)
{
    $handle = opendir($dir);
    while (false !== ($file = readdir($handle))) {
        if ($file != '.' && $file != '..') {
            echo $file . "\n";
            if (filetype($dir . '/' . $file) == 'dir') {
                loopDir($dir . '/' . $file);
            }
        }
    }
}

loopDir($dir);
```

##### 9，会话控制

> 为什么要使用会话控制技术？

思想：允许服务器跟踪同一个客户端做出的连续请求。

> 实现方式：

- 通过GET方式参数传递，信息不安全、参数丢失
- Cookie 存储在客户端(浏览器)的片段信息,不会占用服务器资源，不适用敏感信息;cookie添加setcookie, 获取cookie的值$_COOKIE(),删除cookie：setcookie($name, '', time() - 1)
- Session 存储在服务器，基于cookie;session_start() $_SESSION() 销毁$_SESSION = []、session_destroy()

> 传递sessionID的问题：

```php
<a href="1.php?<?php echo session_name() . '=' . session_id();?>">next page</a>
<a href="1.php?<?php echo SID;?>">next page</a>
```

> session存储，共享
```
session_set_save_handler()方法

mysql,redis.memecache
```

充分理解session和cookie的工作原理。

- session的存储方式，如何进行遍历：默认以文件的方式存储在服务中，还可以通过使用session_set_save_handler()存储到内存中mysql、redis等; 遍历使用$_SEESION.

##### 10，面向对象

> 延伸知识点：面向对象的封装、继承和多态；魔术方法；设计模式。

> PHP类控制权限修饰符

- public : 具有最高权限，在类的内部外部子类使用； 
- protected : 可在类的内部使用，子类中使用但不能在类的外部使用；
- private : 只能在类的内部使用，不能在外部使用不能被继承；

> 面向对象的封装

- 成员访问权限

> 面向对象的继承

- 单一继承
- 方法重写

```php
<?php
class Father
{
    public function test() {};
}

class Son extends Father
{
    // 重写
    public function test()
    {
        // 如果想要扩展父类中的方法
        parent::test();
        ......
    }
}
```

> 面向对象多态

- 抽象类的定义abstract
- 接口的定义

> 魔术方法

```php
__construct()、__destruct()、__call()、__callStatic()、__get()、__set()、__isset()、__unset()、__sleep()、__wakeup()、__toString()、__clone()
```
** 注意：构造函数：1，方法名和类名一致是，这个方法就是构造方法；2，另外一种就是__construct()

> 设计模式

常见的设计模式：
- 工厂模式
- 单例模式
- 注册树模式
- 适配器模式
- 观察者模式
- 策略模式

##### 11，网络协议

> 知识点总结:

- HTTP协议状态码；
- OSI七层模型；
- HTTP协议的工作特点和工作原理；
- HTTP常见请求/响应头和响应方法；
- HTTPS的工作原理；
- 常见网络协议和端口

> HTTP协议状态码

五类响应: 1xx／2xx／3xx／4xx／5xx

- 1开头的：信息类状态码
- 2开头的：success,成功状态码
- 3开头的：redirect,重定向
- 4开头的：client error,客户端错误，服务器无法处理请求
- 5开头的：server error,服务器处理请求出错

常见的状态码：

- 200：ok
- 204: 服务器接收的请求正常处理，但在响应的时候报文中不含实体的主体部分
- 206: 部分内容
- 301：永久重定向
- 302：临时重定向,请求的资源已经分配了新的uri希望用户本次能够使用新的uri
- 303：请求的资源存在另一个uri,应使用get方法定向获取请求的资源
- 304：not modified
- 307：临时重定向
- 400：bad request请求报文中存在语法错误
- 401：发送的请求有要通过http认证的认证信息
- 403：访问的资源被拒绝
- 404: 请求的资源未找到
- 500: 服务器在处理请求时发生错误 
- 503：服务器处于超负载或者正在进行停机维护现在无法处理请求

> OSI七层模型

- 物理层：建立、维护、断开物理连接
- 数据链路层：建立逻辑链接、进行硬件地址寻址、差错校验等功能
- 网络层：进行逻辑地址寻址，实现不同网络之间的路径选择
- 传输层：定义传输数据的协议端口号，以及流程和差错校验；协议有：TCP,UDP，数据包一旦离开网卡即进入网络传输层
- 会话层：建立、管理、终止会话
- 表示层：数据的表示、安全、压缩
- 应用层：网络服务与最终用户的一个接口；协议有：HTTP、FTP、TFTP、SMTP、DNS、TELNET、HTTPS、POP3、DHCP

> HTTP协议的工作特点和工作原理

- 工作特点：基于B/S模式；通信开销小、简单快速、传输成本低；使用灵活、可使用超文本传输协议；节省传输时间；无状态

- 工作原理：客户端发送请求给服务器，创建一个TCP链接，指定端口号，默认80，连接到服务器，服务器监听浏览器请求，一旦监听到客户端请求，分析请求类型后，服务器会向客户端返回状态信息和数据内容

> HTTP常见请求/响应头和响应方法

- Content-Type : 请求的与实体对应的MIME信息
- Accept : 指定客户端能接受的内容
- Origin : 最初的请求来源出处，主要用于post请求
- Cookie : http请求发起时，发送给服务器cookie的值
- Cache-Control : 指定请求和响应的缓存机制
- User-Agent : 用户信息
- Referer : 上级请求路径
- X-Forwarded-For : 请求端真实的IP
- Access-Control-Allow-Origin : 允许特定域名来进行访问，通常用来做跨域访问
- Last-Modified : 请求资源的最后响应时间

> HTTP协议的请求方法

```
GET、POST、PUT、DELETE、HEAD、OPTIONS、TRACE
```

GET和POST的区别？

- GET在做后退操作和刷新时没有太大变化，无害的，POST数据会被重新提交
- GET可以收藏为书签，POST不可能被收藏为书签
- GET请求可以被浏览器缓存，POST不可以
- GET请求编码类型为application/x-www-form-urlencoded，post请求除了改编码外还有multipart/form-data
- GET在历史记录中会保留在浏览器中，POST不会
- GET种数据长度有限制，最大2048字符，POST没有限制
- 数据类型的限制，GET只允许ASCII，POST没限制
- 与POST相比GET的安全性较差

> HTTPS的工作原理

HTTPS是一种基于SSL/TLS的HTTP协议，所有的HTTP数据都是在SSL/TLS协议封装之上传输的。

HTTPS协议在HTTP协议的基础上添加了SSL/TLS握手以及数据加密传输，也属于应用层协议。

> 常见网络协议和端口

- FTP：文件传输协议 默认21
- TELNET：用于远程登录的端口 23默认
- SMTP：简单邮件传输协议 25默认
- POP3：对应SMTP，用来接收邮件 110默认
- HTTP：超文本传输协议 默认80
- DNS：用于域名解析服务 默认53

##### 12，开发环境及配置

> PHP运行原理

- CGI/FastCGI/php-fpm的区别理解
- PHP-FPM是FastCFI的进程管理器 master(一个)/work(多个)进程

通过nginx反向代理代理9000端口

> 常见配置项

```
register_globals、allow_url_fopen、allow_url_include、display_error、date.timezone...
```