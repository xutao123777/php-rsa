# php-rsa
rsa加密技术

(一)：单向散列加密
单向散列加密
散列是信息的提炼, 通常其长度要比信息小得多, 且为一个固定长度。加密性强的散列一定是不可逆的, 这就意味着通过散列结果, 无法推出任何部分的原始信息。
任何输入信息的变化, 哪怕仅一位, 都将导致散列结果的明显变化, 这称之为雪崩效应。散列还应该是防冲突的, 即找不出具有相同散列结果的两条信息。具有这些特性的散列结果就可以用于验证信息是否被修改。
单向散列函数一般用于产生消息摘要, 密钥加密等，常见的有： MD5， SHA。

MD5
简介： Message Digest Algorithm MD5(即消息摘要算法第五版)为计算机安全领域广泛使用的一种散列函数, 用以提供消息的完整性保护。
特点：
压缩性： 任意长度的数据, 算出的 MD5 值长度都是固定的。
容易计算： 从原数据计算出 MD5 值很容易。
抗修改性： 对原数据进行任何改动, 哪怕只修改1个字节, 所得到的 MD5 值都有很大区别。
强抗碰撞： 已知原数据和其 MD5 值, 想找到一个具有相同 MD5 值的数据(即伪造数据)是非常困难的。
应用： 一致性验证, 数字签名, 安全访问认证。
弱点： 目前已经可以暴力破解 MD5 的值了, 即逆推回去。并且从理论上来说 MD5 并不是绝对的唯一, 所以在某些极限条件下还是会出现重复的情况的。


string md5  ( string $str  [, bool $raw_output  = false  ] )
使用 » RSA 数据安全公司的 MD5 报文算法计算 str 的 MD5 散列值。 
参数
str 	   原始字符串。 
raw_output 如果可选的 raw_output 被设置为 TRUE, 那么 MD5 报文摘要将以16字节长度的原始二进制格式返回。 
返回值	以 32 字符十六进制数字形式返回散列值。



SHA-1
SHA(Secure Hash Algorithm, 安全散列算法), SHA-1 属于 SHA 中最常见的一种另外其他还有 SHA-256，SHA-384 和 SHA-512。
特点和应用领域都和 MD5 类似, 区别在于 SHA-1 更加安全, 但是会慢一些。但是目前 Google 已经宣布可以破解了。。。并且即使是 SHA-256, SHA-384 和 SHA-512 也不能完全保证绝对的安全。

算法	    摘要长度   实现方
SHA-1       160        JDK
SHA-224     224        Bouncy Castle
SHA-256     256        JDK
SHA-384		384        JDK
SHA-512		512        JDK


string sha1  ( string $str  [, bool $raw_output  = false  ] )
利用» 美国安全散列算法 1 计算字符串的 sha1 散列值。 
参数
str 	    输入字符串。 
raw_output 	如果可选的 raw_output 参数被设置为 TRUE, 那么 sha1 摘要将以 20 字符长度的原始格式返回, 否则返回值是一个 40 字符长度的十六进制数字。 
返回值		返回 sha1 散列值字符串。 


string hash ( string $algo , string $data [, bool $raw_output = false ] )
hash — 生成哈希值(消息摘要)
algo	要使用的哈希算法, 例如："md5", "sha256", "haval160,4" "sha512"等。
data	要进行哈希运算的消息。
raw_output	设置为 TRUE 输出原始二进制数据, 设置为 FALSE 输出小写 16 进制字符串。
返回值		如果 raw_output 设置为 TRUE, 则返回原始二进制数据表示的信息摘要, 否则返回 16 进制小写字符串格式表示的信息摘要。


string crypt  ( string $str  [, string $salt  ] )
crypt()  返回一个基于标准 UNIX DES 算法或系统上其他可用的替代算法的散列字符串。 
参数
str 	待散列的字符串。 
salt 	可选的盐值字符串。如果没有提供, 算法行为将由不同的算法实现决定, 并可能导致不可预料的结束。 
返回值  返回散列后的字符串或一个少于 13 字符的字符串, 从而保证在失败时与盐值区分开来。 


string password_hash ( string $password , integer $algo [, array $options ])
创建和校验哈希密码  (PHP 5 >= 5.5.0)
password   用户的密码。 
algo       一个用来在散列密码时指示算法的密码算法常量。 
options    一个包含有选项的关联数组。目前支持两个选项：salt, 在散列密码时加的盐(干扰字符串), 以及cost, 用来指明算法递归的层数。这两个值的例子可在 crypt() 页面找到。 
返回哈希密码, 或者在失败时返回 FALSE . 



单向散列加密的 php 代码示例
<?php
/*** md5**/
echo 'md5: ' . PHP_EOL;
var_dump(md5('password string.')); // 32 byte
var_dump(md5('password string.', TRUE)); // 16 byte （默认是16byte的原始二进制格式，会显示乱码，解决方案如下）
var_dump(substr(md5('password string.'), 8, 16)); //16byte乱码解决方案：32byte时的第8到24位之间的内容和16byte情况下的值是一样的，可以截取下就行

/*** sha1**/
echo 'sha1: ' . PHP_EOL;
var_dump(sha1('password string.')); // 40byte
var_dump(sha1('password string.', TRUE)); // 20byte （默认是16byte的原始二进制格式，会显示乱码，解决方案如下）
var_dump(substr(md5('password string.'), 8, 16)); // 20byte乱码解决方案：40byte时的第10到30位之间的内容和20byte情况下的值是一样的，可以截取下就行

/*** hash**/
echo 'hash: ' . PHP_EOL;
var_dump(hash('sha512', 'password string.')); // hash支持多种哈希算法，这里演示‘sha512’方式。512bit，即128byte
var_dump(hash('sha512', 'password string.', TRUE)); // 还支持第三个参数用于输出截取后的原始二进制值，不同算法长度不同

/*** crypt**/
echo 'crypt: ' . PHP_EOL;
var_dump(crypt('password string.')); // 默认创建出来的是弱密码，即没有加盐则会有不同的算法自动提供，php5.6及之后的版本会报个提示
var_dump(crypt('password string.', '$1$rasmusle$')); //加的盐值也应该设置一定的复杂度，并且由于不同系统使用的算法不一致所以可能会导致得到的结果也不一致，可以用默认的crypt来生成盐值
var_dump(hash_equals(crypt('password string.'), crypt('password string.', crypt('password string.'))));
// 这里用默认的crypt来生成盐值，这样就避免了不同算法的问题
// hash_equals比较两个字符串，无论它们是否相等，本函数的时间消耗是恒定的。这里就可以专门用来crypt函数的时序攻击
// 这里再科普一下时序攻击：一句话就是通常比较密码时计算机从头开始按位逐次比较遇见不同就返回false，因为计算速度是一定的，
// 那么等于说可以根据这个计算时间来推断大概多少位的时候开始不一样了，这样就大大降低了破译密码的难度了

/*** password_hash**/
echo 'password_hash: ' . PHP_EOL;
var_dump(password_hash('password string.', PASSWORD_DEFAULT));
// password_hash是crypt的一个简单封装，并且完全与现有的密码哈希兼容，所以推荐使用
//第二个参数指明用什么算法，目前支持：PASSWORD_DEFAULT和PASSWORD_BCRYPT，通常使用前者，目前是60个字符，但是以后可能会增加，所以数据库可以设置成255个字符比较稳妥，后者生成长度为60的兼容使用 "$2y$" 的crypt的字符串
// 第三个参数是加盐，php7之后已经废除了，因为默认会给出一个高强度的盐值
var_dump(password_verify('password string.', password_hash('password string.', PASSWORD_DEFAULT))); 
// password_verify是专门用来比较用password_hash生成的密码的，可以防止时序攻击，可以参考hash_equals之于crypt




对称加密
对称加密(也叫私钥加密)指加密和解密使用相同密钥的加密算法。有时又叫传统密码算法, 就是加密密钥能够从解密密钥中推算出来, 同时解密密钥也可以从加密密钥中推算出来。
而在大多数的对称算法中, 加密密钥和解密密钥是相同的, 所以也称这种加密算法为秘密密钥算法或单密钥算法。它要求发送方和接收方在安全通信之前, 商定一个密钥。
对称算法的安全性依赖于密钥, 泄漏密钥就意味着任何人都可以对他们发送或接收的消息解密, 所以密钥的保密性对通信性至关重要。常见的是：DES 算法。
base64_encode — 使用 MIME base64 对数据进行编码 
说明
string base64_encode  ( string $data  )
使用 base64 对 data 进行编码。 
设计此种编码是为了使二进制数据可以通过非纯 8-bit 的传输层传输，例如电子邮件的主体。 
Base64-encoded 数据要比原始数据多占用 33% 左右的空间。 
参数
data 	要编码的数据。 
返回值  编码后的字符串数据, 或者在失败时返回 FALSE 。


base64_decode — 对使用 MIME base64 编码的数据进行解码 
说明
string base64_decode  ( string $data  [, bool $strict  = false  ] )
对 base64 编码的 data 进行解码。 
参数	
data   编码过的数据。 
strict 如果输入的数据超出了 base64 字母表, 则返回 FALSE 。 
返回值
返回原始数据, 或者在失败时返回 FALSE。返回的数据可能是二进制的。 


/*** urlencode**/
echo 'urlencode: ' . PHP_EOL;
var_dump(urlencode('http://www.baidu.com')); 
// 编码url字符串，此字符串中除了 -_. 之外的所有非字母数字字符都将被替换成百分号（%）后跟两位十六进制数，空格则编码为加号（+）。此编码与 WWW 表单 POST 数据的编码方式是一样的，同时与 application/x-www-form-urlencoded 的媒体类型编码方式一样。与其对应的逆向解密函数为urldecodevar_dump(htmlspecialchars('<a href="test">Test</a>')); 
// 将特殊字符转换为 HTML 实体，与其对应的逆向解密函数为htmlspecialchars_decode。不够用的话用htmlentities，会把所有具有 HTML 实体的字符都转换了。
/*** base64_encode**/
echo 'base64_encode: ' . PHP_EOL;
var_dump(base64_encode('password string.')); 
// 使用 MIME base64 对数据进行编码，设计此种编码是为了使二进制数据可以通过非纯 8-bit 的传输层传输，例如电子邮件的主体。
// Base64-encoded 数据要比原始数据多占用 33% 左右的空间。与其对应的逆向解密函数为base64_decode()



非对称加密
非对称加密为数据的加密与解密提供了一个非常安全的方法, 它使用了一对密钥, 公钥(public key)和私钥(private key)。私钥只能由一方安全保管, 不能外泄，而公钥则可以发给任何请求它的人。
非对称加密使用这对密钥中的一个进行加密, 而解密则需要另一个密钥。比如, 你向银行请求公钥, 银行将公钥发给你, 你使用公钥对消息加密, 那么只有私钥的持有人--银行才能对你的消息解密。
与对称加密不同的是, 银行不需要将私钥通过网络发送出去, 因此安全性大大提高。

见源码


虽然非对称加密安全性更高, 但是速度比较慢, 所以目前最通用的方案是：将对称加密的密钥使用非对称加密的公钥进行加密, 然后发送出去, 接收方使用私钥进行解密得到对称加密的密钥, 
然后双方可以使用对称加密来进行沟通。
