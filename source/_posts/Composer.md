---
title: Composer
copyright: true
date: 2016-10-12 00:38:34
tags:
  - PHP
  - Composer
---

## 安装
```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === 'e115a8dc7871f15d853148a7fbac7da27d6c0030b848d9b3dc09e2a0388afed865e6a3d6b3c0fad45c48e2b5fc1196ae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

# 添加到 PATH
sudo mv composer.phar /usr/local/bin/composer
```

## 设置中国全量源
```bash
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

## 更新Composer
```bash
composer selfupdate
```

## 使用
```bash
# 引入依赖包
composer require doctrine/dbal
```