
## on Triggered 触发

单一事件触发
```yml
on: push
```

## on.<push|pull_request>.<branches|tags>
事件列表
```yml
on: [push, pull_request]
```

多个事件
```yml
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
```

## 特定事件
```yml
on:
  release:
    type: [published, created, edited]
```

## 特定branch/tag
```yml
on:
  push:
    # Sequence of patterns matched against refs/heads
    branches:    
      # Push events on main branch
      - main
      # Push events to branches matching refs/heads/mona/octocat
      - 'mona/octocat'
      # Push events to branches matching refs/heads/releases/10
      - 'releases/**'
    # Sequence of patterns matched against refs/tags
    tags:        
      - v1             # Push events to v1 tag
      - v1.*           # Push events to v1.0, v1.1, and v1.9 tags
```

## 忽略branch/tag
```yml
on:
  push:
    # Sequence of patterns matched against refs/heads
    branches-ignore:
      # Push events to branches matching refs/heads/mona/octocat
      - 'mona/octocat'
      # Push events to branches matching refs/heads/releases/beta/3-alpha
      - 'releases/**-alpha'
    # Sequence of patterns matched against refs/tags
    tags-ignore:
      - v1.*           # Push events to tags v1.0, v1.1, and v1.9
```
branches 或 branches-ignore 无法同时使用
tags 或 tags-ignore 无法同时使用

可以使用 ! 字符排除 tags 和 branches
```yml
# 以下工作流程将在到 releases/10 或 releases/beta/mona 的推送上运行，
# 而不会在到 releases/10-alpha 或 releases/beta/3-alpha 的推送上运行，
# 因为否定模式 !releases/**-alpha 后跟肯定模式
on:
  push:
    branches:    
    - 'releases/**'
    - '!releases/**-alpha'
```


## on.<push|pull_request>.paths
```yml
# 如果至少有一个路径与 paths 过滤器中的模式匹配，工作流程将会运行。 
# 要在每次推送 JavaScript 文件时触发构建，您可以使用通配符模式。
on:
  push:
    paths:
    - '**.js'
```

## 忽略提交文件路径
```yml
# 任何时候路径名称匹配 paths-ignore 中的模式，则工作流程不会运行
on:
  push:
    paths-ignore:
    - 'docs/**'
```

paths 和 paths-ignore 无法同时使用
可以使用 ! 字符排除
只要 push 事件包括 sub-project 目录或其子目录中的文件，
```yml
# 此示例就会运行，除非该文件在 sub-project/docs 目录中。 
# 例如，更改了 sub-project/index.js 或 sub-project/src/index.js 的推送将会触发工作流程运行，
# 但只更改 sub-project/docs/readme.md 的推送不会触发。
on:
  push:
    paths:
    - 'sub-project/**'
    - '!sub-project/docs/**'
```

## on.schedule [schedule](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html#tag_20_25_07)
```yml
# 可以使用 POSIX cron 语法安排工作流程在特定的 UTC 时间运行。 
# 预定的工作流程在默认或基础分支的最新提交上运行。 
# 您可以运行预定工作流程的最短间隔是每 5 分钟一次。

# 每隔 15 分钟触发工作流程
on:
  schedule:
    # * is a special character in YAML so you have to quote this string
    - cron:  '*/15 * * * *'
```
