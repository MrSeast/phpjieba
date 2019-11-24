### windows下编译dll
以编译php-5.6.40 x86为例，如果是要x64，则有x86的地方改为x64即可
1. 下载本phpjieba源码
2. 下载php源码：https://github.com/php/php-src.git/tags/php-5.6.40
3. 下载php sdk：https://windows.php.net/downloads/php-sdk/

   a) php-sdk-binary-tools-20110915.zip
   
   b) archives/deps-5.6-vc11-x86_2018-12.7z
4. 下载安装VS2012
5. 找个地方创建php_sdk目录，将php-sdk-binary-tools-20110915.zip解压到该目录下
6. 打开Developer Command Prompt for VS2012命令行窗口，进入php_sdk目录，如果是编译x64，这里需要打开x64的命令行窗口
7. 运行脚本创建编译目录
```
bin\phpsdk_buildtree.bat phpdev
```
8. 创建目录phpdev\vc11\x86，将deps-5.6-vc11-x86_2018-12.7z解压到该目录下
9. 创建目录phpdev\vc11\x86\php-5.6.40-src，将php源码导入到该目录下
10. 创建目录phpdev\vc11\x86\php-5.6.40-src\ext\jieba，将phpjieba源码导入到该目录下
11. 创建目录phpdev\vc11\x86\obj，作为编译输出目录
12. 开始编译
```
bin\phpsdk_setvars.bat
cd phpdev\vc11\x86\php-5.6.40-src
buildconf
configure --disable-all --enable-cli --enable-zts --enable-jieba=shared --enable-object-out-dir=..\obj
nmake
```
13. 编译完成后就可以在obj目录下找到php_jieba.dll，将其拷贝到你的网站php\ext目录下
14. 编辑php.ini文件，添加以下配置，然后启动网站就可以用了
```
extension=php_jieba.dll
jieba.enable=1
jieba.dict_path="C:\phpjieba\cjieba\dict" ;修改为你自己的路径
```


### version 0.0.6
* 加载字典缘故嫌慢的同学可以考虑使用  https://github.com/jonnywang/goredisjieba

### functions
```php
array jieba(string $text, int $action = 0, int $limit = 50)
```
* action
  * 0 Extract
  * 1 CutForSearch
  * 2 Tag
  * 3 TagAll 解决 Tag 对于相同 key 的问题

### install
```
git clone https://github.com/jonnywang/phpjieba.git
cd phpjieba/cjieba
make

cd ..
phpize
./configure
make
make install
```
* jieba more detail please visit https://github.com/yanyiwu/cppjieba

### php.ini
```
extension=jieba.so
jieba.enable=1
jieba.dict_path=/data/softs/phpjieba/cjieba/dict
```
*jieba.dict_path指向字典所在对应目录，请根据自己编译目录替换更改

### example
```
$result = jieba('小明硕士毕业于中国科学院计算所，后在日本京都大学深造');
echo implode('/', $result) . PHP_EOL;
//计算所/小明/京都大学/深造/硕士/中国科学院/毕业/日本

$result = jieba('小明硕士毕业于中国科学院计算所，后在日本京都大学深造', 1, 50);
echo implode('/', $result) . PHP_EOL;
//小明/硕士/毕业/于/中国/科学/学院/科学院/中国科学院/计算/计算所/，/后/在/日本/京都/大学/京都大学/深造

$result = jieba('他心理健康', 1);
echo implode('/', $result) . PHP_EOL;
//他/心理/健康/心理健康

$result = jieba('this is a demo, my name is jony', 1, 10);
echo implode('/', $result) . PHP_EOL;
//demo/jony

$result = jieba('this is a demo, my name is jony');
echo implode('/', $result) . PHP_EOL;
//this/ /is/ /a/ /demo/,/ /my/ /name/ /is/ /jony

$result = jieba('小明硕士毕业于中国科学院计算所，后在日本京都大学深造', 2);
print_r($result);

Array
(
    [小明] => x
    [硕士] => n
    [毕业] => n
    [于] => p
    [中国科学院] => nt
    [计算所] => n
    [，] => x
    [后] => f
    [在] => p
    [日本] => ns
    [京都大学] => nz
    [深造] => v
)
```
 * 更新请参考example目录
 * 词性可参考HanLP词性标注集解释

### contact
更多疑问请+qq群 233415606 

### plus
https://github.com/jonnywang/goredisjieba

### 贡献者
* https://github.com/imlcl


