---
title: 【工作笔记】edX-h版主题定制
date: 2018-08-16 17:49:23
tags:
- 工作
---

#### 1. Overview
> To override the files that constitute the default Open edX theme, you create replacements for one or more of those files, place them in the file paths that are constructed and named in parallel to the default file locations, configure your Open edX instance to use the files in your theme’s directories instead of the default locations, and then compile the theme.
(EdX first looks for files in your theme directories, and uses any file that matches the exact file path and file name of a default UI file.)

<br>

#### 2. Understanding which UI files to customize 
- UI files for the LMS are stored in the directories shown in the following example theme directory.
```
  /edx-platform/cms/static/images
  /edx-platform/cms/static/sass
  /edx-platform/cms/templates
```

- The default Open edX theme includes an image file name logo.png that appears in the header of most LMS pages. The file path is lms/static/images/logo.png
```
  /edx-platform/cms/static/images/studio-logo.png
```
<br>

#### 3. Enabling and Applying themes 应用主题
1. Create an installation-wide theme directory to hold the themes you create. The directory will hold subdirectories for each theme. You might place it at the root of the file system in a directory named `/my-themes`.
```
  my-themes
   |
   └─ my-theme-red
        ├── README.rst
        ├─── lms
        |     ├── static
        |     |      └── images
        |     |      |     └── logo.png
        |     |      |
        |     |      └── sass
        |     |            └── partials
        |     |                   └── base
        |     |                        └── _variables.scss
        |     |
        |     └── templates
        |             └── footer.html
        |             └── header.html
        └── cms
             ├── static
             |      └── images
             |      |     └── studio-logo.png
             |      |
             |      └── sass
             |            └── partials
             |                   └── _variables.scss
             |
             └── templates
                     └── login.html
```

2. Allow user to read and write files in the themes directory, you might use the following commands.
```
  sudo chown -R edxapp:edxapp /my-themes
  sudo chmod -R u+rw /my-themes
```

3. Enter the configuration file.
```
  cd devstack # 进入devstack目录
  make dev.up # 启动服务器
  docker ps # 查看  edx.devstack.studio 的容器ID
  docker exec -it containerid  /bin/bash # 进入容器
  cd /edx/app/edxapp # 进入文件目录
  LMS	         /edx/app/edxapp/lms.env.json
  Studio       /edx/app/edxapp/cms.env.json
  E-commerce   /edx/etc/ecommerce.yml
```

3. set the `ENABLE_COMPREHENSIVE_THEMING` configuration property to `true`.
```
  "ENABLE_COMPREHENSIVE_THEMING": true.
```

4. Add the absolute path of the themes directory to the `COMPREHENSIVE_THEME_DIRS` configuration property.
```
  "COMPREHENSIVE_THEME_DIRS": [
    "/edx/app/edxapp/edx-platform/themes"
  ],
  "THEME_NAME": "normal-theme"
```

5. Restart all servers.
```
  cd devstack 
  docker-compose restart
```
<br>

#### 4. Compiling a Theme 编译主题
To update a theme for Studio or the LMS, follow these steps.
- Log in to the Open edX machine as the edxapp user.
```
  make lms-shell # 进入studio的shell 以便执行paver 指令
```
- Change to the `/edx/app/edxapp/edx-platform` directory.
- Execute the `paver update_assets` command to update all themes.
- 自动编译sass
```
// 注释加快自动编译速度 execute_webpack_watch(settings=settings)
paver watch_assets --td /edx/app/edxapp/edx-platform/themes --t normal-theme
```
<br>

#### 5. Apply a theme to a site
1. Visit http://0.0.0.0:18010/admin
2. Select Site themes
3. Add site theme
4. From the Site menu, select the site you want to apply a theme to.
5. In the theme dir name field, enter the identifier of the theme
6. Save
```
账号密码：edx/edx

Site:
Domain name: 0.0.0.0:18000
Display name: normal-theme

Theme dir name:
normal-theme
```
<br>

#### 6. 其它
- `make logs`
To see logs from containers running in detached mode

- `make dev.provision`
Recreate up-to-date databases, static assets, etc.

- `make provision` 安装依赖报错
  1. `make lms-shell` to get an edx-platform prompt
  2. In the container, `make clean` to clear the stale files.
  3. `exit` to leave the container
  4. `make lms-restart` to restart the LMS

<br>
参考资料
- [Changing Themes for an Open edX Site](https://edx.readthedocs.io/projects/edx-installing-configuring-and-running/en/latest/configuration/changing_appearance/theming/index.html)