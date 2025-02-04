# 使用 php:8.3.8-fpm-alpine 作为基础镜像
FROM php:8.3.8-fpm-alpine

# 安装常见的系统依赖包
RUN apk --no-cache add \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libzip-dev \
    curl \
    git \
    bash \
    tzdata

# 安装 docker-php-extension-installer 脚本
ADD --chmod=0755 https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/install-php-extensions

# 复制 .ext 文件到容器中
COPY ./php/.ext /tmp/.ext

# 设置工作目录
WORKDIR /www

# 安装扩展
RUN if [ -f /tmp/.ext ]; then \
      cat /tmp/.ext | while read ext; do \
        if [ ! -z "$$ext" ]; then \
          echo "Installing extension: $$ext"; \
          install-php-extensions $$ext; \
        fi; \
      done; \
    fi && rm -rf /tmp/.ext

# 安装 Composer
RUN install-php-extensions @composer

# 清理缓存
RUN apk del tzdata && rm -rf /var/cache/apk/*

# 启动 PHP-FPM
CMD ["php-fpm"]
