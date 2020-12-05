# 基于Hexo的博客文件 bogerv.github.io

确保本地已经安装了

- Node 12.*
- Hexo

## 0x00.本机部署

- 安装Hexo Search插件

    > 如果用到本地搜索也要安装 hexo-generator-search, 并且要安装到项目的根目录
    >
    > `npm install hexo-generator-search --save`

- 安装相应主题

    ```zsh
    cd bogerv.github.io/themes
    git clone https://github.com/bogerv/hexo-theme-cactus.git cactus
    ```

- 在根目录运行相应的hexo命令即可

    ```sh
    hexo clean
    hexo generator
    hexo server
    hexo deploy
    ```

## 0x01.Docker部署

> [https://github.com/bogerv/docker-hexo.git](https://github.com/bogerv/docker-hexo.git)

- 安装 Docker

- 下载镜像

    `docker pull spurin/hexo`

- 创建容器

    ```sh
    docker create --name=blog.me \
    -e HEXO_SERVER_PORT=4000 \
    -e GIT_USER="git用户名" \
    -e GIT_EMAIL="git@邮箱.com" \
    -v /data/github/blog.me:/app \
    -p 127.0.0.1:4000:4000 \
    spurin/hexo
    ```

- 启动容器

    `docker start blog.me`

    `docker logs --follow blog.me`

- 安装主题

    ```sh
    docker exec -it blog.me bash
    cd /app
    git clone https://github.com/bogerv/hexo-theme-cactus.git themes/cactus
    ```

- 重新启动

    ```sh
    exit
    docker restart blog.me
    ```

- 其他命令

    ```sh
    docker exec -it blog.me hexo generate
    docker exec -it blog.me hexo deploy
    sudo nginx -s reload
    ```

## 0x02.配置 Nginx 反向代理

- 安装 Nginx
- 编辑配置文件 `vim /etc/nginx/conf.d/blog.me.conf`

    ```conf
    server {
        listen       80;
        server_name  blog.me;

        location / {
            proxy_pass http://0.0.0.0:4000;
            index  index.html index.htm index.jsp;
        }
    }

    server {
        listen       80;
        server_name  www.blog.me;

        location / {
            proxy_pass http://0.0.0.0:4000;
            index  index.html index.htm index.jsp;
        }
    }
    ```

- 重新启动 Nginx `sudo systemctl restart nginx.service`
