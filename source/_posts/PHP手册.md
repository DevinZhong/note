---
title: PHP手册
copyright: true
date: 2016-09-20 21:05:07
tags: PHP
---

## 待整理
- 自 PHP 5.4 起，短格式的 echo 标记 <?= 总会被识别并且合法，而不管 short_open_tag 的设置是什么。
- `strpos()` 查询不到返回 `false`
- `var_dump()` 打印值和类型
- `gettype()` 返回类型
- `settype()` 亦可转换类型
- `true` 和 `false` 不区分大小写
- 字符串 `"0"` 和 空数组转 `boolean` 类型，值为 `false`
- 字符串 `"false"` 转 `boolean` 类型，值为 `true`
- `ord()` 和 `chr()` ASCII 与字符间转换
- `list(, $secondElement) = [1, 2, 3]` 有点类似于解构，待验证
- `count()` 计算数组长度
- `array_diff()` 用于比较数组，看来也可以用飞船运算符，还有数组运算符
- 数据可以在元素赋值时自动创建
- 数组默认传值，非引用
- `resource` 类型的变量保存到外部资源的引用
- 变量尚未赋值时为 `NULL`
- `0 == “a”` 的值为 `true`
- `expr1 ?: expr3` 当 `expr1` 为 `true` 返回 `expr1` 的值，否则返回 `expr3` 的值
- 激活 `track_errors` 特性时，错误信息将自动存放在 `$php_errormsg` 中，新的错误信息将覆盖旧的信息，所以尽早检查
- 反引号不能在双引号字符串中使用
- 越是腐败的国家越喜欢反腐，越是制度不完善的国家越喜欢谈价值观。
- 递增/递减运算符不影响布尔值。递减 NULL 值也没有效果，但是递增 NULL 值的结果是1。
- `$a = 'Z'; $a++;` 将把 `$a` 变成 `AA`
- `PHP_EOL`
- `or` 和 `and` 的优先级甚至比 `=` 还低
- `$a = new MyClass; $b = new MyClass; var_dump($a instanceof $b);` 输出 `true`
- 析构函数即使在使用 `exit()` 终止脚本运行时也会被调用。在析构函数中调用 `exit()` 将会中止其余关闭操作的运行。
- 试图在析构函数（在脚本终止时被调用）中抛出一个异常会导致致命错误。
- 不能在 `__toString()` 方法中抛出异常。这么做会导致致命错误。
- 属性不能被定义为 final，只有类和方法才能被定义为 final。
- `clone` 对象执行浅复制（shallow copy）。所有的引用属性 仍然会是一个指向原来的变量的引用。
- 在非静态环境下，所调用的类即为该对象实例所属的类。由于 $this-> 会在同一作用范围内尝试调用私有方法，而 static:: 则可能给出不同结果。另一个区别是 static:: 只能用于静态属性。
- 静态属性不能通过类实例访问，而静态方法可以。
- 静态成员不能通过 `->` 运算符访问。
- 在声明命名空间之前唯一合法的代码是用于定义源文件编码方式的 declare 语句。 `declare(encoding='UTF-8');`
- 类名称总是解析到当前命名空间中的名称。因此在访问系统内部或不包含在命名空间中的类名称时，必须使用完全限定名称。
- 一个生成器不可以返回值： 这样做会产生一个编译错误。然而return空是一个有效的语法并且它将会终止生成器继续执行。
- 如果具有引用的数组被拷贝，其值不会解除引用。对于数组传值给函数也是如此。待考究。
- 把 global $var; 当成是 $var =& $GLOBALS['var']; 的简写。从而将其它引用赋给 $var 只改变了本地变量的引用。


## API
- `$fileHandle = fopen($filename, 'r')`
- `fseek($fileHandle, 0)`
- `$line = fgets($fileHandle)`
- `fclose($fileHandle)`
- `array_key_exists('b', $b)`
- `property_exists($c, 'd')`

## 大小写
- 变量名区分大小写，函数名和类名不区分。
- 常量名默认区分大小写，通常大写。
- 魔术常量不区分大小写，推荐大写。`__LINE__`、`__FILE__`、`__CLASS__`
- `NULL`、`TRUE`、`FALSE` 不区分大小写

## 代码片段
```php
class MyClass {}

$a = new MyClass;
$b = new MyClass;
$c = 'MyClass';

var_dump($a instanceof $b); // true
var_dump($a instanceof $c); // true
```

```php
class SomeClass {}
interface SomeInterface {}
trait SomeTrait {}

var_dump(new class(10) extends SomeClass implements SomeInterface {
    private $num;

    public function __construct($num)
    {
        $this->num = $num;
    }

    use SomeTrait;
});

/**
 * 输出：
 * object(class@anonymous)#1 (1) {
 *   ["Command line code0x104c5b612":"class@anonymous":private]=> int(10)
 * }
 */
```

```php
迭代器遍历输出：
/**
rewinding
current: value 1
valid: 1
current: value 1
key: 0
key/value: [0 -> value 1]

next: value 2
current: value 2
valid: 1
current: value 2
key: 1
key/value: [1 -> value 2]

next: value 3
current: value 3
valid: 1
current: value 3
key: 2
key/value: [2 -> value 3]

next:
current:
valid:
*/
```

```php
<?php
class A {
    public static function foo() {
        static::who();
    }

    public static function who() {
        echo __CLASS__."\n";
    }
}

class B extends A {
    public static function test() {
        A::foo();
        parent::foo();
        self::foo();
    }

    public static function who() {
        echo __CLASS__."\n";
    }
}
class C extends B {
    public static function who() {
        echo __CLASS__."\n";
    }
}

C::test();
?>

/**
 * A
 * C
 * C
 */
```

```php
<?php
namespace namespacename;
class classname
{
    function __construct()
    {
        echo __METHOD__,"\n";
    }
}
function funcname()
{
    echo __FUNCTION__,"\n";
}
const constname = "namespaced";

include 'example1.php';

$a = 'classname';
$obj = new $a; // prints classname::__construct
$b = 'funcname';
$b(); // prints funcname
echo constant('constname'), "\n"; // prints global

/* note that if using double quotes, "\\namespacename\\classname" must be used */
$a = '\namespacename\classname';
$obj = new $a; // prints namespacename\classname::__construct
$a = 'namespacename\classname';
$obj = new $a; // also prints namespacename\classname::__construct
$b = 'namespacename\funcname';
$b(); // prints namespacename\funcname
$b = '\namespacename\funcname';
$b(); // also prints namespacename\funcname
echo constant('\namespacename\constname'), "\n"; // prints namespaced
echo constant('namespacename\constname'), "\n"; // also prints namespaced
?>
```
