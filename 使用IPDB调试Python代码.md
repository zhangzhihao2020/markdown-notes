# 使用IPDB调试Python代码
## 安装与使用
IPDB以Python第三方库的形式给出，使用`pip install ipdb`/`pip install ipdb --user`即可轻松安装。
###  集成到源代码中
通过在代码开头导入包，可以直接在代码指定位置插入断点。如下所示：
```
import ipdb
# some code
x = 10
ipdb.set_trace()
y = 20
# other code
```
则程序会在执行完x = 10这条语句之后停止，展开Ipython环境，就可以自由地调试了。
### 命令式
上面的方法很方便，但是也有不灵活的缺点。对于一段比较棘手的代码，我们可能需要按步执行，边运行边跟踪代码流并进行调试，这时候使用交互式的命令式调试方法更加有效。启动IPDB调试环境的方法也很简单：
```
python -m ipdb your_code.py
```
## 常用命令
`h`--可调出IPDB的帮助。可以使用help command的方法查询特定命令的具体用法。
`n`--(next)执行下一条语句。
`s`--(step into)进入函数调用的内部。
`b line_numbe`--指定的行号位置加上断点。
`cl/clear file:line_number`--清除所有断点。
`cl 断点号`--清除指定断点。
`b file_name:line_number`--给指定的文件（还没执行到的代码可能在外部文件中）中指定行号位置打上断点。
`c`--(continue)执行代码直到遇到某个断点或程序执行完毕。
`r`--(return)执行代码直到当前所在的这个函数返回。
`j line_number`--(jump)可以跳过某段代码，直接执行指定行号所在的代码。
`l first[, second]`--查看到更多的上下文代码，first指示向上最多显示的行号，second指示向下最多显示的行号（可以省略）。当second小于first时，second指的是从first开始的向下的行数（相对值vs绝对值）。
`w`--(where)可以打印出目前所在的行号位置以及上下文信息。
`whatis variable_name`--查看变量的类别（感觉有点鸡肋，用type也可以办到）。
`type(variable_name)`--查看变量的类别。
`a`--(argument)身处一个函数内部的时候,打印出传入函数的所有参数的值.
`p`--可以打印表达式的值。
`restart`--重新启动调试器，断点等信息都会保留。restart实际是run的别名，使用run args的方式传入参数。
`q`--退出调试，并清除所有信息。

