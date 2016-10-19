---
title: git pull时保存或删除凭证
date: 2016-10-19 17:25:50
---

#### 保存Git凭证:
`git config --local credential.helper
store`
#### 删除已经保存的凭证:
`git config --local credential.helper
remove`