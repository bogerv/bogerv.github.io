---
title: git pull时保存或删除凭证
date: 2016-10-19 17:25:50
categories: [git]
---

- 设置用户名邮箱

    ```git
    git config --system user.name "bogerv"
    git config --system user.email "bogerv@163.com"
    ```

- 设置别名

    ```git
    git config --global alias.st status
    git config --global alias.co checkout
    git config --global alias.br branch
    git config --global alias.ci commit
    git config --global alias.st status
    git config --global alias.mg merge
    git config --global alias.last "log -1 HEAD"
    ```

- 保存Git凭证

    ```git
    git config --local credential.helper store
    ```

- 删除已经保存的凭证

    ```git
    git config --local credential.helper remove
    ```

- 同步已删除分支

    ```git
    git remote prune origin
    ```

- 解决github图片不显示问题

    ```git
    # 修改 hosts 文件
    # windows: C:\Windows\System32\drivers\etc
    # Mac: `sudo vim /etc/hosts`
    # GitHub Start
    192.30.253.112    github.com
    192.30.253.119    gist.github.com
    151.101.184.133    assets-cdn.github.com
    151.101.184.133    raw.githubusercontent.com
    151.101.184.133    gist.githubusercontent.com
    151.101.184.133    cloud.githubusercontent.com
    151.101.184.133    camo.githubusercontent.com
    151.101.184.133    avatars0.githubusercontent.com
    151.101.184.133    avatars1.githubusercontent.com
    151.101.184.133    avatars2.githubusercontent.com
    151.101.184.133    avatars3.githubusercontent.com
    151.101.184.133    avatars4.githubusercontent.com
    151.101.184.133    avatars5.githubusercontent.com
    151.101.184.133    avatars6.githubusercontent.com
    151.101.184.133    avatars7.githubusercontent.com
    151.101.184.133    avatars8.githubusercontent.com
    # GitHub End
    ```
