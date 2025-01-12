# OP-Plus
A magical OPNET (Riverbed) Modeler model conversion tool, help you to enjoy a more modern OPNET development experience.

[[English]](https://github.com/ZacharyJia/opp/blob/master/README.md) [[中文]](https://github.com/ZacharyJia/opp/blob/master/README-CN.md)


> [!WARNING]
> OP-Plus will modify your model files. Please make sure to backup your files before using.
Although the software provides automatic model backup, we cannot 100% guarantee this tool is bug-free. **Users are responsible for any losses caused by using this tool**.


https://github.com/user-attachments/assets/9480659b-ff3f-4799-9ab0-da5b2c702c73


## 1. Introduction
OP-Plus is an extension for OPNET (Riverbed Modeler) process models, designed to convert the code in OPNET process model files (.pr.m) into plain-text files. This enables users to view, edit, and analyze the code using modern IDEs (such as VSCode and CLion) and utilize advanced features like **code suggestions** and **Copilot support**. The goal of OP-Plus is to significantly improve the development efficiency of OPNET process models, making the process more convenient and productive.

## 2. Motivation
OPNET is a network simulation software, and its process models are core parts of the simulation engine. While OPNET's process models are written in C, they are stored as binary files (code segments for state entry, state exit, functions, etc.) to support graphical development. This restricts code viewing and editing to OPNET's built-in (and very outdated) editor, preventing the use of modern IDEs. This limitation severely reduces development efficiency.

Furthermore, because the process models are stored in binary form, they cannot be managed with version control tools. This makes it impossible to track changes, manage versions, or enable collaborative development, complicating maintenance and development workflows. These challenges are far below the standards of modern software development practices.

To address these issues, we developed OP-Plus, which converts OPNET process model code into plain-text files. This allows users to edit, analyze, and manage the code using modern IDEs and version control tools. OP-Plus also enables advanced IDE features like code suggestions and Copilot integration, ultimately streamlining the development and management of OPNET process models.

## 3. Features
- Converts OPNET process model code into plain-text files.
- Supports the conversion of any process model, including standard OPNET process models, with no specific limitations.
- Code modifications in the plain-text files can be compiled and run directly in OPNET without requiring re-conversion back.
- Compatible with IDEs such as VSCode and CLion.
- Enables IDE features such as code suggestions, syntax highlighting, and autocompletion.
- Supports version control tools like Git for managing versions and collaborative development.
- Integrates with advanced IDE features like Copilot.

## 4. Usage

### 4.1 Installation and Usage
[Click here to download the latest version](https://github.com/ZacharyJia/opp/releases)

OP-Plus is a command-line tool, with a precompiled executable named `opp.exe`.

After downloading, you can copy it to the OPNET installation directory, for example: `C:\OPNET\14.5.A\sys\pc_amd_win64\bin`. Note that only a 64-bit executable is provided, so it must be placed in the `pc_amd_win64` directory.

To use OP-Plus, execute the following command:
```shell
opp.exe --model <full model path> [options]
```

Parameters:
- `<full model path>`: The full path to the OPNET process model file.
- `[options]`: Optional parameters:
  - `--update-sv`: Updates only the state variables.
  - `--update-state [state-name]`: Updates only the code for a specific state.
  - `--no-backup`: Disables the default automatic backup before conversion. **Use with caution.**
  - `--help`: Displays help information.

Without any options, OP-Plus will convert the OPNET process model file into plain-text files. The converted files will be stored in a folder with the same name as the original file, located in the same directory. The folder will typically include:
- `header_block.h`: Contains the header block code from the process model.
- `function_block.cpp`: Contains the function block code from the process model.
- `sv.h`: Contains the state variable (sv) code. **Note: This code is read-only. To modify sv, use OPNET's process editor. Changes made here will not take effect.**
- `tv.h`: Contains the temporary variable (tv) code, which can be modified.
- `diag_block.cpp`: Contains the diagnostic block code from the process model.
- `term_block.cpp`: Contains the termination block code from the process model.
- `state_[state-name]_enter.cpp`: Contains the state entry code for a specific state.
- `state_[state-name]_exit.cpp`: Contains the state exit code for a specific state.

After conversion, you can use IDEs like VSCode or CLion to view, edit, and analyze the code. Modified code can be directly compiled and run in OPNET without re-conversion.

### 4.2 Recommended Workflow
1. For models that have never been converted before, perform a full conversion without any options (**Do not convert the same model multiple times, as it may result in code loss**).
2. Writing codes using the plain-text files.
3. Perform all other operations within OPNET’s graphical interface.
4. After modifying the code, compile it in the OPNET graphical interface, then run the simulation.
5. If you modify the state variables (SV) in OPNET, update them using the command:
   ```shell
   opp.exe --model <model path> --update-sv
   ```
6. If you add new states to the state machine in OPNET, update them using the command:
   ```shell
   opp.exe --model <model path> --update-state <new state name>
   ```
7. If you delete states from the state machine in OPNET, manually delete the corresponding `state_[state-name]_enter.cpp` and `state_[state-name]_exit.cpp` files, or simply leave them untouched to avoid accidental deletions.

## 5. IDE Support
OP-Plus supports IDEs like VSCode and CLion, enabling features such as code viewing, editing, and analysis. Configuration steps are provided below.

### 5.1 CLion
Create a `CMakeLists.txt` file in the root directory of the model folder, with the following content:
```cmake
cmake_minimum_required(VERSION 3.17)
project(OPNET)

set(CMAKE_CXX_STANDARD 98)  # Set C++ standard to C++98 for compatibility with VS2010. For VS2013 or later, use C++11.

# Add header file search paths. Include each model folder without the .pr.m suffix.
include_directories(.)
include_directories(<model_name1>)
include_directories(<model_name2>)

# Add a magic definition.
add_definitions(-DNSE_VSC_MAGIC_DEF)

# Add system header file search paths. Adjust according to the actual OPNET installation path.
include_directories("C:/Riverbed/18.6/sys/include" "C:/Riverbed/18.6/models/std/include")

# Add third-party library headers (optional).
#include_directories("thirdparty/libzmq-prebuilt-4.3.5/include")

# Add all files from model directories to the SRC_LIST variable.
aux_source_directory(<model_name1> SRC_LIST)
aux_source_directory(<model_name2> SRC_LIST)

# Add related files from the current directory to SRC_LIST. Include all files you want to edit in CLion.
list(APPEND SRC_LIST
        xxx.c
)

# Compile files in SRC_LIST into a dummy executable named "models" (for IDE suggestions only; no actual compilation).
add_executable(models ${SRC_LIST})
```
Adjust the `CMakeLists.txt` file according to your specific requirements.

After creating the file, open the entire model folder in CLion to edit, analyze, and manage the code.

### 5.2 VSCode
**To be completed...**

## 6. Notes
1. OP-Plus modifies your model files. Always back up your files before using it. Although automatic backups are provided, we cannot guarantee 100% bug-free. **Users are responsible for any loss caused by this tool.**
2. **Do not convert the same model multiple times**, as it may result in code loss. While OP-Plus has mechanisms to prevent duplicate conversions, exercise caution.
3. After modifying a model in the IDE, manually open the corresponding `.pr.m` file in OPNET, click the compile button, and then run the simulation. (A more automated solution is in development.)
4. Close the process model editor in OPNET before running OP-Plus to avoid conflicts caused by simultaneous modifications.
5. If you need to use Chinese characters (including comments) in the code files, change the file encoding to GB2312 or GBK to avoid issues.
6. The software is free to download and use but is closed-source and does not provide commercial support.

## 7. Donations
If you find this tool useful, feel free to make a donation! 🥳🥳🥳

![WeChat Pay](https://github.com/user-attachments/assets/dc08faa6-5612-4da4-8ac6-972541318cd9)
![Alipay](https://github.com/user-attachments/assets/874c0c46-f7e5-40ce-a598-5899b261bb24)

