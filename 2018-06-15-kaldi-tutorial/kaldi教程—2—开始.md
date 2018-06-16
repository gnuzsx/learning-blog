### kaldi教程——2——开始(15 minutes)

第一步是下载并安装kaldi。

我们将使用该工具包的第1版，以便本教程不会过时。

但是，请注意，`trunk`(始终是最新的)中的代码和脚本更容易安装，并且通常更好。

如果使用`trunk`代码，您还可以尝试使用最新的脚本，这些脚本位于目录`egs/rm/s5`中，

而不是本教程中提到的`s3`脚本。

但是，请注意，如果您这样做，教程的某些方面可能已过期。

假设安装了git，以获取可以键入的最新代码

```
git clone https://github.com/kaldi-asr/kaldi.git kaldi-trunk --origin golden
```

然后cd到`kaldi-trunk`。看看INSTALL文件并按照说明(它指向两个子目录)。

仔细看看安装脚本的输出，因为他们试图指导你做什么。

一些安装错误是非致命的，并且安装脚本会告诉你这样。

即有一些它安装的东西是很好的，但是不是真正需要的。

**最好的情况**是你做的

```
cd kaldi-trunk/tools;

make;

cd ../src;

./configure;

make

```

但是，如果没有发生这种情况，则会有备份计划。

```

例如，您可能需要在计算机上安装一些软件包，或者在tools/中运行install_atlas.sh，

或者手动运行工具/INSTALL中的一些步骤，或者为配置脚本提供选项在 src/。

如果有问题，在构建过程中可能会有一些信息(如何编译kaldi)，这将有助于您；

否则，请随时联系维护人员（其他kaldi相关资源以及如何获得帮助），

我们将很乐意提供帮助。

```

### kaldi教程-3-版本控制与git(5分钟)

http://kaldi-asr.org/doc/tutorial_git.html

### kaldi教程-4-发行概况(20分钟)

在我们跳入示例脚本之前，让我们花几分钟时间查看kaldi发行版中的其他内容。

转到kaldi-1目录并list所有内容。

有几个文件和子目录。

重要的子目录是"tools/"，"src/"和"egs/"，

我们将在下一节中看到。

我们将概述"tools/" 和 "src/"。

#### tools/目录(10分钟)

目录`tools`是我们以各种方式安装kaldi所依赖的东西的地方，

切换到目录tools并list所有内容，

您将看到各种文件和子目录，大部分是由make命令安装的内容。

文件INSTALL提供如何安装这些工具的说明。

最重要的子目录是OpenFst的子目录。

cd到openfst/。

这是一个具有版本号的实际目录的软链接。

列出openfst目录。

如果安装成功，将会有一个带有已安装的二进制文件的bin/目录，

以及带有库的lib/目录（我们需要这两个目录）。

最重要的代码是include/fst/目录。

如果您想了解kaldi，您将需要了解openFst。

为此，最好的出发点是http://www.openfst.org 

现在，只需要查看文件include/fst/fst.h。

这包括抽象FST类型的一些声明。

你可以看到有很多模板涉及。

如果不熟悉这些模板，你可能会在理解这个代码的时候遇到麻烦。

切换到目录bin/，或将其添加到路径中。

我们将从这里执行一些简单的示例命令。

将以下命令粘贴到shell中。

```

# arc格式：src dest ilabel olabel [weight]
# 最后一行的格式: state [weight]
# 每一行可能以任何顺序发生，除了初始状态必须是第一行
# 未指定的权重默认为0.0(对于library-default权重类型)
cat >text.fst << EOF
0 1 a x .5
0 1 b y 1.5
1 2 c z 2.5
2 3.5
EOF

```

以下命令创建符号表; 将它们粘贴到shell中。

```
cat >isyms.txt <<EOF

<eps> 0

a 1

b 2 

c 3 

EOF
```
```
cat >osyms.txt <<EOF

<eps> 0

x 1

y 2

z 3

EOF
```
注意：要使以下步骤正常工作，如果您的PATH上没有当前目录，可能需要输入：

```

export PATH=.$PATH

```

接下来创建一个二进制格式的FST：

```

fstcompile -isymbols=isyms.txt --osymbols=osyms.txt text.fst binary.fst 

```

我们来执行一个例子命令：

```

fstinvert binary.fst | fstcompose - binary.fst > binary2.fst 

```

所得到的WFST，binary2.fst应该与binary.fst相似，但weights的两倍。

你可以打印出来，看看:

```
fstprint --isymbols=isyms.txt --osymbols=osyms.txt binary.fst 
fstprint --isymbols=isyms.txt --osymbols=osyms.txt binary2.fst 
```

该示例从www.openfst.org上提供的更长的教程进行了修改。

完成此操作后，请输入以下内容：

```
rm *.fst *.txt -f 
```