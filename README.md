# OP-Plus
一款神奇的 OPNET(Riverbed) Modeler 模型转换工具，享受更现代的 OPNET 开发体验

> [!WARNING]  
> **注意！**  
> OP-Plus会修改你的模型文件，使用过程中请务必做好备份。  
> 尽管软件本身已经提供了自动模型备份，但我们无法100%保证本工具没有Bug，**使用本工具带来的一切损失由使用人自行负责。**



https://github.com/user-attachments/assets/9480659b-ff3f-4799-9ab0-da5b2c702c73


## 1. 简介
OP-Plus是一个针对OPNET(Riverbed Modeler)的进程模型的扩展，主要作用是将OPNET的进程模型文件 (.pr.m) 中的代码转换为纯文本文件形式的代码，以便于使用IDE（VSCode、CLion）进行代码的查看、编辑、分析等操作，并支持其他更高级的功能（**代码提示**、**Copilot支持**等）。OP-Plus的目标是大幅度提高OPNET的进程模型的开发效率，使得OPNET的进程模型开发更加便捷、高效。


## 2. Motivation
OPNET是一个用于网络仿真的软件，其进程模型是OPNET的核心部分，是OPNET的仿真引擎。OPNET的进程模型使用C语言编写，但是为了实现基于图形化界面的开发，OPNET的进程模型代码是以二进制文件的形式存储的代码段（状态进入代码、状态退出代码、函数代码等等），这使得OPNET的进程模型代码只能在OPNET的（非常原始的）代码编辑器界面中进行查看、编辑，无法直接使用IDE进行代码的查看、编辑、分析等操作，大大降低了进程模型的开发效率。

此外，二进制文件的形式也使得OPNET的进程模型代码无法使用版本控制工具进行版本管理，进而无法实现代码的版本追踪、多人协作开发等功能。这使得OPNET的进程模型代码的开发、维护、管理等工作变得非常困难，大大落后于现代软件开发的标准。

为了能够更好地开发、维护、管理OPNET的进程模型代码，我开发了OP-Plus，将OPNET的进程模型代码转换为纯文本文件形式的代码，以便于使用IDE进行代码的查看、编辑、分析等操作，并支持其他更高级的功能（代码提示、Copilot支持等），也使得OPNET的进程模型代码可以使用版本控制工具进行版本管理，进而实现代码的版本追踪、多人协作开发等功能。最终目标是大幅度提高OPNET的进程模型的开发效率，使得OPNET的进程模型开发更加便捷、高效。

## 3. 主要功能
- 通过模型转换，将OPNET的进程模型代码转换为纯文本文件形式的代码
- 支持任意进程模型的转换，包括OPNET标准进程模型，无特殊限制
- 在文本文件中修改代码后，无需进行代码转换，可以直接在OPNET中编译运行
- 支持VSCode、CLion等IDE
- 支持IDE的代码提示、代码高亮和补全等功能
- 支持使用git等版本控制工具进行版本管理
- 支持IDE的Copilot等高级功能

## 4. 使用

### 4.1 安装与使用
[点击查找最新版本下载](https://github.com/ZacharyJia/opp/releases)

OP-Plus是一个命令行工具，预编译的可执行文件为`opp.exe`。
下载完成后可以直接拷贝到OPNET安装目录下，例如：`C:\OPNET\14.5.A\sys\pc_amd_win64\bin`。注意我们只提供了64-bit的可执行程序，因此必须放到pc_amd_win64目录下。

使用方法如下：
```shell
opp.exe --model <full model path> [options]
```

其中：
- `<full model path>`为OPNET的进程模型文件的完整路径，
- `[options]`为可选参数，具体参数如下：
  - `--update-sv`: 仅更新状态变量
  - `--update-state [state-name]`: 仅更新指定状态的代码
  - `--no-backup`: OPP默认会在转换前备份原始文件，使用此选项可以禁止备份，使用时请注意风险
  - `--help`: 显示帮助信息

在不添加任何参数的情况下，OP-Plus会将OPNET的进程模型文件转换为纯文本文件形式的代码，转换后的文件会保存在原始文件的同一目录下创建一个同名的文件夹，文件夹中包含了转换后的代码文件，通常包括：
- `header_block.h`: 包含了进程模型中的header block代码
- `function_block.cpp`: 包含了进程模型中的function block代码
- `sv.h`: 包含了进程模型中的sv代码，**注意：这部分代码是只读的，要修改sv请在OPNET的进程编辑器中修改，在此处的修改不生效。**
- `tv.h`: 包含了进程模型中的tv代码，这部分是可以修改生效的
- `diag_block.cpp`: 包含了进程模型中的diag block代码
- `term_block.cpp`: 包含了进程模型中的termination block代码
- `state_[state-name]_enter.cpp`: 包含了进程模型中指定状态的状态进入代码
- `state_[state-name]_exit.cpp`: 包含了进程模型中指定状态的状态退出代码

在转换后，可以使用VSCode、CLion等IDE打开转换后的文件夹，进行代码的查看、编辑、分析等操作。在修改代码后，可以直接在OPNET中编译运行，无需进行代码转换。

### 4.2 常见工作流
1. 对于从来没有转换过的模型，执行一次不加任何options的完整转换（**务必不要对同一模型进行重复转换，会丢代码**）；
2. 对模型进行开发，所有代码工作都可以在转换之后的纯文本文件中进行。
3. 所有其他操作仍在OPNET的图形界面中进行；
4. 修改代码之后，如果要生效，请在OP图形界面中点击编译，然后运行仿真；
5. 如果在OP中修改了进程模型的SV，则执行更新：opp.exe --models <model path> **--update-sv**；
6. 如果在OP中的状态机中增加了新的状态，则执行更新：opp.exe --models <model path> **--update-state <新增加的状态名>**；
7. 如果在OP中的状态机中删除了原有的状态，可以手动操作删除掉 对应的 `state_[state-name]_enter.cpp` 和 `state_[state-name]_exit.cpp` 文件，也可以无需理会，避免误删文件。


## 5. IDE支持
OP-Plus支持VSCode、CLion等IDE，可以使用这些IDE进行代码的查看、编辑、分析等操作，也支持这些IDE的代码提示、代码高亮和补全等功能，但是需要进行一些配置。

### 5.1 CLion

在整个模型目录的根目录下创建一个`CMakeLists.txt`文件，内容如下：
```cmake
cmake_minimum_required(VERSION 3.17)
project(OPNET)

set(CMAKE_CXX_STANDARD 98)  # 设置C++标准为C++98，适配VS2010等，VS2013及以上可以使用C++11

# 添加头文件搜索路径，建议每个模型产生的文件夹都添加进来，无pr.m后缀
include_directories(.)
include_directories(<模型名1>)
include_directories(<模型名2>)


# 添加一个神奇的定义
add_definitions(-DNSE_VSC_MAGIC_DEF)

# 添加系统头文件搜索路径，默认在C盘，根据实际OPNET安装位置调整
include_directories("C:/Riverbed/18.6/sys/include" "C:/Riverbed/18.6/models/std/include")

# 添加第三方库的头文件 （Optional，如果需要第三方库支持的话则添加）
#include_directories("thirdparty/libzmq-prebuilt-4.3.5/include")

# 将各目录下的所有文件都添加到变量SRC_LIST中
aux_source_directory(<模型名1> SRC_LIST)
aux_source_directory(<模型名2> SRC_LIST)

# 将当前目录下的相关文件都添加到变量SRC_LIST中，其他想用Clion编辑的文件也都加入到这里来
list(APPEND SRC_LIST
        xxx.c
)


# 将SRC_LIST中的文件编译成一个名为models的可执行文件（只是为了IDE提示用的，并不会实际编译）
add_executable(models ${SRC_LIST})

```
CMakeLists.txt的文件可以根据实际情况进行调整。

创建完成后，使用CLion打开整个模型目录，即可使用CLion进行代码的查看、编辑、分析等操作。


### 5.2 VSCode

To be completed...

## 6. 注意事项
1. OP-Plus会修改你的模型文件，使用过程中请务必做好备份。尽管软件本身已经提供了自动模型备份，但我们无法100%保证本工具没有Bug，**使用本工具带来的一切损失由使用人自行负责。**
2. 务必不要对同一模型进行重复转换，**会丢代码**。尽管软件本身已经提供了防止重复转换的机制，但是千万不要手贱。
3. 每次在IDE中修改了某个模型，需要手动打开对应的pr.m文件，点击编译按钮编译，之后再运行仿真。（有解决方案，但是没时间整理，敬请期待）
4. 每次执行OPP命令的时候，都建议关掉对应进程模型的编辑器，以防出现两方同时修改模型文件导致的冲突。
5. 如果要在代码文件中使用中文（包括注释），建议将文件编码改为GB2312或GBK，避免出现问题。
6. 软件目前提供免费下载和使用，但不开源，无商业支持。

## 7. 捐款
如果有用的话可以扫码捐款 🥳🥳🥳

<img src="https://github.com/user-attachments/assets/dc08faa6-5612-4da4-8ac6-972541318cd9" width="200" alt="Wechat Pay" />
<img src="https://github.com/user-attachments/assets/874c0c46-f7e5-40ce-a598-5899b261bb24" width="200" alt="Alipay" />
