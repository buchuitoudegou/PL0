# PL0报告

## 实验内容
1. 任务一：修改编译程序和源程序，通过输入文件输入PL0源程序，在输出文件中产生源程序的中间代码，然后运行该中间代码，然后运行该中间代码，在输出文件中产生数据。
2. 任务二：修改编译程序，可以在键盘输入数据，向文件（屏幕）写输入数据。

## PART1
### 步骤一
修改所有的中文标点符号。这一部分要很有耐心，一边通过编译器报错，一边检查有哪些中文符号。尤其是中文的“减号”，肉眼很难识别。

然后还要将所有的pascal的保留字替换成非保留字。有`object`,`procedure`，但是不能用全局替换，因为`procedure`在pascal的代码中代表函数的声明。

### 步骤二
删除所有的goto语句。查看代码逻辑之后，发现goto语句是没有用的。

### 步骤三
运算符修正。代码中出现了几个神奇的符号，都是计算机语言中没有用过的，分别是`≠`, `≤`, `≥`。一开始打算在PL0程序中进行替换，使用这几个特殊符号来编译程序。在编译的时候可以发现，这几个符号不是标准的ASCII编码，在读入的时候会识别成两个编码（估计是unicode的编码）。最后还是用了pascal中用到的`<>`等符号代表。需要加入这样的代码：
```pascal
Else If ch = '<'
         Then
         Begin
           getch;
           If ch = '='
             Then
             Begin
               sym := leq;
               getch
             End
           Else If ch = '>'
                  Then
                  Begin
                    sym := neq;
                    getch
                  End
           Else sym := lss
         End
  Else If ch = '>'
         Then
         Begin
           getch;
           If ch = '='
             Then
             Begin
               sym := geq;
               getch
             End
           Else sym := gtr
         End
```
### 语法修改
将这些内容改为小写
```
word[1] := 'begin        ';
word[2] := 'call         ';
word[3] := 'const        ';
word[4] := 'do           ';
word[5] := 'end          ';
word[6] := 'if           ';
word[7] := 'odd          ';
word[8] := 'procedure    ';
word[9] := 'then         ';
word[10] := 'var          ';
word[11] := 'while        ';
```

### 逻辑修改
```
While Not eoln(fin) Do
-------------------
Procedure test( s1,s2 :symset; n: integer );
Begin
  If Not ( sym In s1 )
    Then
    Begin
      error(n);
      s1 := s1+s2;
      While Not( sym In s1) Do
        getsym
    End
End;
---------------------
Until Not (sym In declbegsys);
---------------------
If Not (sym In [eql, neq, lss, leq, gtr, geq]) Then
```
这里是忘了加not的语句。以上语句一开始都是没有not的。
```
num := 10*num + (ord(ch)-ord('0'));
```
这里要给0加上单引号，代表字符0，一开始是没有的。

## PART2
有空写
