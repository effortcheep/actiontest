## job 工作流程
工作流程运行包括一项或多项作业。 作业默认是并行运行。 要按顺序运行作业，您可以使用 <job_id>needs 关键词在其他作业上定义依赖项

每个作业在 runs-on 指定的环境中运行

## jobs.<job_id>
每项作业必须关联一个 ID。 键值 job_id 是一个字符串，其值是作业配置数据的映像。 必须将 <job_id> 替换为 jobs 对象唯一的字符串。 <job_id> 必须以字母或 _ 开头，并且只能包含字母数字字符、- 或 _
```yml
jobs:
  my_first_job:
    name: My first job
  my_second_job:
    name: My second job
```

## jobs.<job_id>.needs
```yml
# 在此示例中，job1 必须在 job2 开始之前成功完成，而 job3 要等待 job1 和 job2 完成。
# 此示例中的作业按顺序运行：
# 1. job1
# 2. job2
# 3. job3
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
```

在这个例子中，job3使用了always()条件表达式，因此它总是在job1和job2完成后运行，无论它们是否成功。有关详细信息，请参见 "上下文和表达式语法"。
```yml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    if: always()
    needs: [job1, job2]
```

## jobs.<job_id>.runs-on
必需运行作业的机器类型。 机器可以是 GitHub 托管的运行器或自托管的运行器。

| 虚拟环境      | YAML 工作流程标签 |
| :----:      |   :----:  |
| Windows Server 2019 | windows-latest 或 windows-2019 | 
| Ubuntu 20.04 | ubuntu-20.04 | 
| Ubuntu 18.04 | ubuntu-latest 或 ubuntu-18.04 | 
| Ubuntu 16.04  | ubuntu-16.04 | 
| macOS Big Sur 11.0 | macos-11.0 | 
| macOS Catalina 10.15 | macos-latest 或 macos-10.15 | 
```yml
runs-on: ubuntu-latest
```

## jobs.<job_id>.steps
作业包含一系列任务，称为 steps。 步骤可以运行命令、运行设置任务，或者运行您的仓库、公共仓库中的操作或 Docker 注册表中发布的操作。 并非所有步骤都会运行操作，但所有操作都会作为步骤运行。 每个步骤在运行器环境中以其自己的进程运行，且可以访问工作区和文件系统。 因为步骤以自己的进程运行，所以步骤之间不会保留环境变量的更改。 GitHub 提供内置的步骤来设置和完成作业
## jobs.<job_id>.steps.name
```yml
name: Greeting from Mona

on: push

jobs:
  my-job:
    name: My Job
    runs-on: ubuntu-latest
    steps:
    - name: Print a greeting
      env:
        MY_VAR: Hi there! My name is
        FIRST_NAME: Mona
        MIDDLE_NAME: The
        LAST_NAME: Octocat
      run: |
        echo $MY_VAR $FIRST_NAME $MIDDLE_NAME $LAST_NAME.
```
## jobs.<job_id>.steps.uses
- 公共镜像 {owner}/{repo}@{ref}
```yml
uses: actions/heroku@1.0.0
```

```yml
steps:    
  # Reference a specific commit
  - uses: actions/setup-node@74bc508
  # Reference the major version of a release
  - uses: actions/setup-node@v1
  # Reference a minor version of a release
  - uses: actions/setup-node@v1.2
  # Reference a branch
  - uses: actions/setup-node@main
```

## jobs.<job_id>.steps.run
使用操作系统 shell 运行命令行程序。 如果不提供 name，步骤名称将默认为 run 命令中指定的文本

每个 run 关键词代表运行器环境中一个新的进程和 shell。 当您提供多行命令时，每行都在同一个 shell 中运行

- 单行命令：
```yml
- name: Install Dependencies
  run: npm install
```
- 多行命令：
```yml
- name: Clean install dependencies and build
  run: |
    npm ci
    npm run build
```
使用 working-directory 关键词，您可以指定运行命令的工作目录位置。
```yml
- name: Clean temp directory
  run: rm -rf *
  working-directory: ./temp
```

使用指定 shell/bash/python
```yml
steps:
  - name: Display the path
    run: echo $PATH
    shell: bash
```

## jobs.<job_id>.steps.env
设置供步骤用于运行器环境的环境变量。 您也可以设置整个工作流程或某个作业的环境变量
```yml
steps:
  - name: My first action
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      FIRST_NAME: Mona
      LAST_NAME: Octocat
```
