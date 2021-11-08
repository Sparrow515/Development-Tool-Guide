# 为 Laravel 项目正确设置目录权限

### 设置项目所有者

- `Ubuntu` 

​	设置目录权限为自己（默认是 `ubuntu`）或者 `Webserver` （假设是 `www-data`）

- `CentOS`

​	设置目录权限为非 root 账户或者 `Webserver`

```shell
$ sudo chown -R www-data:www-data /path/to/your/laravel
```

### 添加用户到所有者用户组

如果项目的所有者是 `Webserver`，在使用 sftp 等服务时，会被登录账户覆盖，所以还需要将登录账户添加进 web 服务的用户组。

```shell
$ sudo usermod -a -G www-data ubuntu
```

#### 设置目录和文件的权限

- `Webserver` 为所有者

  ```shell
  # 设置文件权限
  sudo find /path/to/your/laravel/ -type f -exec chmod 644 {} \;
  # 设置目录权限
  sudo find /path/to/your/laravel/ -type d -exec chmod 775 {} \;
  ```

- 个人用户为所有者

  ```shell
  # 跳转到项目所在根目录
  cd /path/to/your/laravel/
  # 设置个人用户为所有者
  sudo chown -R $USER:www-data .
  # 设置文件和目录权限
  sudo find . -type f -exec chmod 664 {} \;
  sudo find . -type d -exec chmod 775 {} \;
  # 给网站服务器读写存储和缓存的权限
  sudo chgrp -R www-data storage bootstrap/cache
  sudo chmod -R ug+rwx storage bootstrap/cache
  ```