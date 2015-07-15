# CoffeeScript 编码风格指南

这份指南阐述了一些 [CoffeeScript][coffeescript] 的最佳实践和编码惯例。

这份指南是社群驱动的，非常鼓励大家来贡献内容。

请注意这还是一份正在完善的指南：仍有很多地方可以改进，有些已制定的准则也不一定是社区惯用的（基于此，在适当的情况下，这些有待斟酌的准则将有可能被修改或删除。）

## 灵感

本指南中的很多细节受到了几份现有的风格指南和其他资源的启发。特别是：

- [PEP-8][pep8]: Style Guide for Python Code
- Bozhidar Batsov's [Ruby Style Guide][ruby-style-guide]
- [Google's JavaScript Style Guide][google-js-styleguide]
- [Common CoffeeScript Idioms][common-coffeescript-idioms]
- Thomas Reynolds' [CoffeeScript-specific Style Guide][coffeescript-specific-style-guide]
- Jeremy Ashkenas' [code review][spine-js-code-review] of [Spine][spine-js]
- The [CoffeeScript FAQ][coffeescript-faq]

## 目录

* [CoffeeScript 风格指南](#guide)
    * [代码布局（Code Layout）](#code_layout)
        * [Tab 还是 空格？（Tabs or Spaces?）](#tabs_or_spaces)
        * [最大行宽（Maximum Line Length）](#maximum_line_length)
        * [空行（Blank Lines）](#blank_lines)
        * [结尾空白（Trailing Whitespace）](#trailing_whitespace)
        * [可选的逗号（Optional Commas）](#optional_commas)
        * [编码（Encoding）](#encoding)
    * [模块导入（Module Imports）](#module_imports)
    * [表达式和语句中的空白（Whitespace in Expressions and Statements）](#whitespace)
    * [注释（Comments）](#comments)
        * [块注释（Block Comments）](#block_comments)
        * [行内注释（Inline Comments）](#inline_comments)
    * [命名规范（Naming Conventions）](#naming_conventions)
    * [函数（Functions）](#functions)
    * [字符串（Strings）](#strings)
    * [条件判断（Conditionals）](#conditionals)
    * [循环和列表解析（Looping and Comprehensions）](#looping_and_comprehensions)
    * [扩展本地对象（Extending Native Objects）](#extending_native_objects)
    * [异常（Exceptions）](#exceptions)
    * [注解（Annotations）](#annotations)
    * [其他（Miscellaneous）](#miscellaneous)

<a name="code_layout"/>
## 代码布局（Code Layout）

<a name="tabs_or_spaces"/>
### Tab 还是 空格？（Tabs or Spaces?）

只用 **空格**，每级缩进均为 **2 个空格**。切勿混用 Tab 和空格。

<a name="maximum_line_length"/>
### 最大行宽（Maximum Line Length）

限制每行最多 79 个字符。

<a name="blank_lines"/>
### 空行（Blank Lines）

顶级函数和类的定义用一个空行分开。

类内部的函数定义也用一个空行分开。

对于每个函数体内，只在为了提高可读性的情况下才使用一个空行（例如：为了达到划分逻辑的目的）。

<a name="trailing_whitespace"/>
### 结尾空白（Trailing Whitespace）

不要在任何一行保留行尾空白。

<a name="optional_commas"/>
### 可选的逗号（Optional Commas）

当对象（或数组）的属性（或元素）作为单独一行列出时，避免在换行符前使用逗号。如下：

```coffeescript
# 好
foo = [
  'some'
  'string'
  'values'
]
bar:
  label: 'test'
  value: 87

# 差
foo = [
  'some',
  'string',
  'values'
]
bar:
  label: 'test',
  value: 87
```

<a name="encoding"/>
### 编码（Encoding）

UTF-8 是首选的源文件编码。

<a name="module_imports"/>
## 模块导入（Module Imports）

如果需要导入模块 (CommonJS 模块，AMD，等等.), `require` 语句应该单独作为一行。如下：

```coffeescript
require 'lib/setup'
Backbone = require 'backbone'
```

这些语句应该按以下顺序去分组：

1. 标准库的导入 _（如果标准库存在）_
2. 第三方库的导入
3. 本地导入 _（导入这个应用程序的或库的具体依赖）_

<a name="whitespace"/>
## 表达式和语句中的空白（Whitespace in Expressions and Statements）

下列情况应该避免多余的空格：

- 紧贴着圆括号、方括号和大括号内部

    ```coffeescript
       ($ 'body') # 好
       ( $ 'body' ) # 差
    ```

- 紧贴在逗号前

    ```coffeescript
       console.log x, y # 好
       console.log x , y # 差
    ```

额外建议：

- 在下列二元操作符的左右两边都保留 **一个空格**

    - 赋值运算符: `=`

        - _注意这同样适用于函数定义中的默认参数_

           ```coffeescript
           test: (param = null) -> # 好
           test: (param=null) -> # 差
           ```

    - 自增运算符: `+=`, `-=`, 等等。
    - 比较运算符: `==`, `<`, `>`, `<=`, `>=`, `unless`, 等等。
    - 算术运算符: `+`, `-`, `*`, `/`, 等等。

    - _（这些操作符两边的空格不要多于一个）_

        ```coffeescript
           # 好
           x = 1
           y = 1
           fooBar = 3

           # 差
           x      = 1
           y      = 1
           fooBar = 3
        ```

<a name="comments"/>
## 注释（Comments）

如果你修改了一段已有注释说明的代码，则也要更新它对应的注释。（理想状态是，重构这段代码直到它不需要注释说明，然后再把之前的注释全删掉。）

注释的首字母要大写，除非第一个单词是以小写字母开头的标识符。

如果注释很短，可以省略末尾的句号。

<a name="block_comments"/>
### 块注释（Block Comments）

注释块通常应用于尾随其后的一段代码。

每一行注释都以 `#` 加一个空格开头，而且和被注释的代码有相同的缩进层次。

注释块内的段落以仅含单个 `#` 的行分割。

```coffeescript
  # 这是一个块注释。请注意假如这是一段块注释，
  # 则它描述的就应该是接下来的这段代码。
  #
  # 这是块注释的第二段。
  # 请注意这段是由上一行带有 # 号的空行分开的。（P.S. 最好用英文写注释）

  init()
  start()
  stop()
```

<a name="inline_comments"/>
### 行内注释（Inline Comments）

行内注释紧贴在被描述的代码的上一行，如果行内注释足够短，则可以处在同一行行尾（由一个空格隔开）。

所有行内注释都以 `#` 加一个空格开头。

应该限制行内注释的使用，因为它们的存在通常是一个代码异味的标志。

不要给显而易见的情况作行内注释：

```coffeescript
  # 差
  x = x + 1 # x 自增
```

然而，行内注释在某些情况下是有用的：

```coffeescript
  # 好
  x = x + 1 # 边界补足
```

<a name="naming_conventions"/>
## 命名规范（Naming Conventions）

使用 `小驼峰命名法` （第一个词的首字母小写，后面每个词的首字母大写）来命名所有的变量、方法和对象属性。

使用 `大驼峰命名法` （第一个词的首字母，以及后面每个词的首字母都大写）来命名所有的类 _（在[其他类似的命名法][camel-case-variations]中，这种风格通常也被称为 `帕斯卡命名法（PascalCase）`、 `大写驼峰命名法（CamelCaps）` 或 `首字母大写命名法（CapWords）`。）_

_（CoffeeScript **官方**  约定是用驼峰命名法，因为这可以简化与 JavaScript 的相互转化，想了解更多，请看[这里][coffeescript-issue-425].)_

对于常量，单词全部大写，用下划线隔开即可：

```coffeescript
CONSTANT_LIKE_THIS
```

私有函数和私有变量都应该在前面加一个下划线：

```coffeescript
_privateMethod: ->
```

<a name="functions"/>
## 函数（Functions）

_（以下这些准则同样适用于类中的方法。）_

当声明一个带参函数时，应在参数列表的右圆括号后空出一个空格：

```coffeescript
foo = (arg1, arg2) -> # 好
foo = (arg1, arg2)-> # 差
```

无参函数不要用圆括号：

```coffeescript
bar = -> # 好
bar = () -> # 差
```

当函数链式调用，却在一行放不下时，则把每个函数调用都另起一行，且都缩进一级（即在 `.` 前加两个空格）。

```coffeescript
[1..3]
  .map((x) -> x * x)
  .concat([10..12])
  .filter((x) -> x < 11)
  .reduce((x, y) -> x + y)
```

当调用函数时，我们应该为了提高可读性而去掉圆括号。请记住，「可读性」是我们主观臆断的。只有类似下面几个例子的情况才被社区认为是最佳的：

```coffeescript
baz 12

brush.ellipse x: 10, y: 20 # 大括号在适当的时候也可以去掉

foo(4).bar(8)

obj.value(10, 20) / obj.value(20, 10)

print inspect value

new Tag(new Value(a, b), new Arg(c))
```

有时候你会发现圆括号用来包裹的是函数体（而不是函数的参数）。请看下面的例子（以下简称为「函数体风格」）：

```coffeescript
($ '#selektor').addClass 'klass'

(foo 4).bar 8
```

这段代码会编译为：

```coffeescript
$('#selektor').addClass 'klass'

foo(4).bar 8
```

一些习惯链式调用的人会巧用「函数体风格」进行单独初始化：

```coffeescript
($ '#selektor').addClass('klass').hide() # 单独初始化调用
(($ '#selektor').addClass 'klass').hide() # 全部调用
```

「函数体风格」并不得到推荐。但是， **当它适应一些特殊的项目需求时，还是得用它。**

<a name="strings"/>
## 字符串（Strings）

用字符串插值代替字符串连接符：

```coffeescript
'this is an #{adjective} string' # 好
'this is an ' + adjective + ' string' # 差
```

最好用单引号 (`''`) 而不是双引号 (`""`) 。除非是插入到另一段现有的字符串中（类似字符串插值）。

<a name="conditionals"/>
## 条件判断（Conditionals）

用 `unless` 来代替 `if` 的否定情况。

不要用 `unless...else`， 而用 `if...else`:

```coffeescript
  # 好
  if true
    ...
  else
    ...

  # 差
  unless false
    ...
  else
    ...
```

多行的 if/else 语句应该缩进：

```coffeescript
  # 好
  if true
    ...
  else
    ...

  # 差
  if true then ...
  else ...
```

<a name="looping_and_comprehensions"/>
## 循环和列表解析（Looping and Comprehensions）

尽可能的使用列表解析：

```coffeescript
  # 好
  result = (item.name for item in array)

  # 差
  results = []
  for item in array
    results.push item.name
```

还可以过滤结果：

```coffeescript
result = (item for item in array when item.name is "test")
```

遍历对象的键值：

```coffeescript
object = one: 1, two: 2
alert("#{key} = #{value}") for key, value of object
```

<a name="extending_native_objects"/>
## 扩展本地对象（Extending Native Objects）

不要修改本地对象。

比如，不要给 `Array.prototype` 引入 `Array#forEach` 。

<a name="exceptions"/>
## 异常（Exceptions）

不要抑制异常抛出。

<a name="annotations"/>
## 注解（Annotations）

必要的时候应该写注解，来指明接下来的代码块具体将干什么。

注解应紧贴在被描述代码的上一行。

注解关键字后面应该跟一个冒号加一个空格，加一个描述性的注释。

```coffeescript
  # FIXME: The client's current state should *not* affect payload processing.
  resetClientState()
  processPayload()
```

如果注解不止一行，则下一行缩进两个空格。

```coffeescript
  # TODO: Ensure that the value returned by this call falls within a certain
  #   range, or throw an exception.
  analyze()
```

注解有以下几类：

- `TODO`: 描述缺失的功能，以便日后加入
- `FIXME`: 描述需要修复的代码
- `OPTIMIZE`: 描述性能低下，或难以优化的代码
- `HACK`: 描述一段值得质疑（或很巧妙）的代码
- `REVIEW`: 描述需要确认其编码意图是否正确的代码

如果你必须自定义一个新的注解类型，则应该把这个注解类型记录在项目的 README 里面。

<a name="miscellaneous"/>
## 其他（Miscellaneous）

`and` 更优于 `&&`.

`or` 更优于 `||`.

`is` 更优于 `==`.

`not` 更优于 `!`.

`or=` 应在可能的情况下使用：

```coffeescript
temp or= {} # 好
temp = temp || {} # 差
```

最好用 (`::`) 访问对象的原型：

```coffeescript
Array::slice # 好
Array.prototype.slice # 差
```

最好用 `@property` 而不是 `this.property`.

```coffeescript
return @property # 好
return this.property # 差
```

但是，避免使用 **单独的** `@`:

```coffeescript
return this # 好
return @ # 差
```

没有返回值的时候避免使用 `return` ，其他情况则需要显示 return 。

当函数需要接收可变数量的参数时，使用 splats  (`...`)。

```coffeescript
console.log args... # 好

(a, b, c, rest...) -> # 好
```

[coffeescript]: http://jashkenas.github.com/coffee-script/
[coffeescript-issue-425]: https://github.com/jashkenas/coffee-script/issues/425
[spine-js]: http://spinejs.com/
[spine-js-code-review]: https://gist.github.com/1005723
[pep8]: http://www.python.org/dev/peps/pep-0008/
[ruby-style-guide]: https://github.com/bbatsov/ruby-style-guide
[google-js-styleguide]: http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml
[common-coffeescript-idioms]: http://arcturo.github.com/library/coffeescript/04_idioms.html
[coffeescript-specific-style-guide]: http://awardwinningfjords.com/2011/05/13/coffeescript-specific-style-guide.html
[coffeescript-faq]: https://github.com/jashkenas/coffee-script/wiki/FAQ
[camel-case-variations]: http://en.wikipedia.org/wiki/CamelCase#Variations_and_synonyms
