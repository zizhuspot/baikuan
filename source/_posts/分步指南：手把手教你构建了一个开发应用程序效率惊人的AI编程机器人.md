---
title: 分步指南：手把手教你构建了一个开发应用程序效率惊人的AI编程机器人
date: 2023-08-22 11:49:31
categories:
  - AI
tags:
  - ChatGPT
  - 编程coding
  - 分步指南
description: 本文旨在手把手教你构建了一个开发应用程序效率惊人的人工智能驱动的编程助手。
cover: https://s2.loli.net/2023/08/22/1W48zY5LBl7fhFG.png
---
![](https://s2.loli.net/2023/08/22/dJ94zr3TUSevIYC.webp)

如果我告诉你，我在 ChatGPT-4 的帮助下编写了一个应用程序，它利用 ChatGPT OpenAI API 一次就能为我编写任何应用程序，你会怎么想？

不信？

我们开始吧 🔥

在当今快节奏的世界里，自动化正占据着中心位置，编程也不例外。你有没有想过，如果有一个私人 Python 机器人，可以为你构建任何应用程序，而且是交钥匙工程，那会是什么样子？这个梦想即将成真。在本文中，我将带您了解开发应用程序的过程，利用 OpenAI 的 ChatGPT 的强大功能，创建一个专家级编程机器人，它可以根据您的需求、架构和详细设计生成整个应用程序。

## 高级架构
应用程序分为多个模块，通过无缝互动提供完整的编程解决方案：

- 应用程序设计
- 代码生成
- 代码执行
- 错误处理
- 测试

从提取信息到生成代码再到执行代码，每个模块都在整个流程中发挥着至关重要的作用。


```
1. 1. App Design
   1.1. Gather User Input
       - Output: user_story, intended_functionality, features, user_groups, key_requirements
   1.2. Generate Requirements (generate_requirements)
       - Input: user_story, intended_functionality, features, user_groups, key_requirements
       - Output: requirements
   1.3. Generate Architecture (generate_architecture)
       - Input: requirements
       - Output: architecture
   1.4. Generate Detailed Design (generate_detailed_design)
       - Input: requirements, architecture
       - Output: detailed_design
   1.5. Save Design Document
       - Input: requirements, architecture, detailed_design
       - Output: design_document.txt

2. Code Generation
   2.1. Load Design Document
       - Input: design_document.txt
       - Output: requirements, architecture, detailed_design
   2.2. Generate Code (generate_code)
       - Input: requirements, architecture, detailed_design
       - Output: .py files

3. Code Execution
   3.1. Run Application (run_application)
       - Input: .py files
       - Output: execution_results, errors (if any)

4. Error Handling and Iteration
   4.1. Apply Bugfixes (apply_bugfixes)
       - Input: .py files, errors
       - Output: updated .py files (with bugfixes)
   4.2. Iterate until code runs successfully

5. Testing
   5.1. Run Tests (run_tests)
       - Input: .py files
       - Output: test_results, test_errors (if any)
```

## 步骤 1：应用程序设计
### 需求、架构和详细设计

应用程序设计模块负责收集用户需求，并根据这些需求设计应用程序。用户需要提供用户故事、预期功能、特性、用户群和关键需求。该模块使用 ChatGPT API 生成应用程序需求、架构和详细设计。然后，这些信息将存储在设计文档（如 design_document.txt）中，作为代码生成过程的输入。

**（gather_user_input.py）：**
```
import uuid

def gather_user_input():
    user_story = input("Please provide the user story for your application: ")
    intended_functionality = input("What is the intended functionality of your application? ")
    features = input("List the main features of your application: ")
    user_groups = input("Describe the intended user groups for your application: ")
    key_requirements = input("List the key requirements of your application: ")

    unique_id = uuid.uuid4()
    design_doc = f"{unique_id}_design_document.txt"
    
    with open(design_doc, "w") as file:
        file.write(f"User Story: {user_story}\n")
        file.write(f"Intended Functionality: {intended_functionality}\n")
        file.write(f"Features: {features}\n")
        file.write(f"User Groups: {user_groups}\n")
        file.write(f"Key Requirements: {key_requirements}\n")

    return unique_id
```

**（design_generation.py）：**
```
import openai
from openai_api_key import api_key

openai.api_key = api_key

def generate_requirements(user_input):
    prompt = f"Generate requirements for an application with the following user input:\n{user_input}"
    response = openai.Completion.create(engine="text-davinci-002", prompt=prompt, max_tokens=100)
    requirements = response.choices[0].text.strip()
    return requirements

def generate_architecture(requirements):
    prompt = f"Generate architecture for an application with the following requirements:\n{requirements}"
    response = openai.Completion.create(engine="text-davinci-002", prompt=prompt, max_tokens=100)
    architecture = response.choices[0].text.strip()
    return architecture

def generate_detailed_design(requirements, architecture):
    prompt = f"Generate detailed design for an application with the following requirements and architecture:\nRequirements:\n{requirements}\n\nArchitecture:\n{architecture}"
    response = openai.Completion.create(engine="text-davinci-002", prompt=prompt, max_tokens=200)
    detailed_design = response.choices[0].text.strip()
    return detailed_design
```

## 步骤 2：代码生成

代码生成模块使用设计文档中的信息为应用程序生成 Python 代码。它利用 ChatGPT API 根据需求、架构和详细设计生成代码。生成的代码会保存到 .py 文件中。

```
import openai
from openai_api_key import api_key

openai.api_key = api_key

def generate_code(requirements, architecture, detailed_design):
    prompt = f"Generate Python code for an application based on the following requirements, architecture, and detailed design:\nRequirements:\n{requirements}\n\nArchitecture:\n{architecture}\n\nDetailed Design:\n{detailed_design}\n\nPlease provide the code as a seasoned Python developer."
    response = openai.Completion.create(engine="text-davinci-002", prompt=prompt, max_tokens=300)
    code = response.choices[0].text.strip()
    return code
```

## 步骤 3：代码执行

代码生成后，代码执行模块负责运行应用程序。它检查执行结果，并识别可能发生的任何错误。

**（code_execution.py）：**
```
import subprocess

def run_application(py_files):
    execution_results = {}
    errors = {}
    for file_name, file_contents in py_files.items():
        try:
            output = subprocess.check_output(["python", file_name], input=file_contents, text=True)
            execution_results[file_name] = output
        except subprocess.CalledProcessError as e:
            errors[file_name] = str(e)
    return execution_results, errors
```

## 步骤 4：错误处理

如果在代码执行过程中遇到错误，错误处理模块将负责修复这些错误。它获取有问题的代码、错误日志和其他相关信息，并使用 ChatGPT API 进行错误修复。这个过程会反复循环执行，直到所有错误修复都得到解决。

**（error_handling.py）：**
```
import openai
from openai_api_key import api_key

openai.api_key = api_key

def apply_bugfixes(py_files, errors):
    updated_py_files = {}
    for file_name, error in errors.items():
        prompt = f"Fix the following error in the Python code:\nError:\n{error}\n\nCode:\n{py_files[file_name]}"
        response = openai.Completion.create(engine="text-davinci-002", prompt=prompt, max_tokens=300)
        updated_code = response.choices[0].text.strip()
        updated_py_files[file_name] = updated_code
    return updated_py_files
```

## 步骤 5：测试
最后一个模块是测试，负责对生成的应用程序进行测试。它检查应用程序的功能，并验证应用程序是否按预期运行。

```
not implemeted yet
```

## 步骤 6：主程序
主应用程序是整个机器人编程过程的指挥者。它读取设计文档，提取需求、架构和详细设计的相关部分。然后，它将这些信息传递给不同模块中的相应功能，确保每个步骤之间的流程顺畅。从生成代码到执行代码再到处理错误，主程序都会全程监督，为无缝编程体验铺平道路。

它将反复重新运行代码生成，直到不再发生错误为止。请确保您设置了执行次数限制，这样在解决方案没有收敛的情况下，您就不会陷入无限期的 while 循环中。

请注意，我们为设计文档设置了一个唯一 ID。因此，您可以随时返回到较早的设计，在该设计上重新运行主程序。

**（main_application.py）：**
```
from gather_user_input import gather_user_input
from design_generation import generate_requirements, generate_architecture, generate_detailed_design
from code_generation import generate_code
from code_execution import run_application
from error_handling import apply_bugfixes
import uuid



def main():
    choice = input("Do you want to create a new design document or use an existing one? (new/existing): ")

    if choice.lower() == "new":
        unique_id = gather_user_input()
    else:
        unique_id = input("Please provide the unique ID of the existing design document: ")

    design_doc = f"{unique_id}_design_document.txt"

    with open(design_doc, "r") as file:
        lines = file.readlines()

    user_story = lines[0].split(": ")[1].strip()
    intended_functionality = lines[1].split(": ")[1].strip()
    features = lines[2].split(": ")[1].strip()
    user_groups = lines[3].split(": ")[1].strip()
    key_requirements = lines[4].split(": ")[1].strip()

    # Generate requirements, architecture, and detailed design
    requirements = generate_requirements((user_story, intended_functionality, features, user_groups, key_requirements))
    architecture = generate_architecture(requirements)
    detailed_design = generate_detailed_design(requirements, architecture)

    # Generate Python code
    code = generate_code(requirements, architecture, detailed_design)

    # Save code to .py file
    with open("generated_app.py", "w") as file:
        file.write(code)

    # Run the application
    py_files = {"generated_app.py": code}
    execution_results, errors = run_application(py_files)

    while errors:
        # Apply bugfixes if needed
        updated_py_files = apply_bugfixes(py_files, errors)
        # Run the updated application
        execution_results, errors = run_application(updated_py_files)

    print("Application executed successfully!")

if __name__ == "__main__":
    main()
```

有了主应用程序，我们的编程机器人就可以随时接受任何挑战，将您的想法转化为功能齐全的应用程序，而您只需付出最少的努力。

## 总结与展望
显然，这只是第一个原型。它能满足更有限的应用需求。

**局限性：**

- 生成代码的令牌可能用完。为了增加稳健性，您可以扩展代码，使 ChatGPT 只提供模块代码，这样您就可以限制其响应的长度，使其不超过令牌限制。
- 您可能想要实现一个功能，使用 ChatGPT 更广泛地分析它生成的代码，并为您提供有关弱点和潜在问题的报告。
- 您可能想根据编码最佳实践编写一本 "游戏手册"，在创建代码时加以遵循，使其符合您的质量标准和偏好。
- ....

通过利用 ChatGPT 的强大功能，我们开发出了一个专业的编程机器人，它可以为您构建任何应用程序。这种开创性的编程方法有可能彻底改变我们开发软件的方式，使我们能够更快地创建应用程序，并减少错误。

想象一下这种可能性：从构建定制的网络应用程序到自动执行数据分析任务，一切尽在掌握。编程的新时代将带来前所未有的创新和生产力，这对开发人员来说是一个激动人心的时刻。

你对这个编程机器人专家有什么看法？您是否准备好一试身手？