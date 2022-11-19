FROM ubuntu
RUN chmod 1777 /tmp
RUN apt-get update -y

RUN ln -snf /usr/share/zoneinfo/$CONTAINER_TIMEZONE /etc/localtime && echo $CONTAINER_TIMEZONE > /etc/timezone
RUN apt-get install tzdata -y

RUN apt-get install git -y
RUN apt-get install software-properties-common -y
RUN git config --global init.defaultBranch main
RUN apt-get install curl -y
RUN apt-get install wget -y
RUN apt-get install unzip -y

RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.3/zsh-in-docker.sh)" -- \
    -t https://github.com/denysdovhan/spaceship-prompt \
    -a 'SPACESHIP_PROMPT_ORDER=(' \
    -a '  dir' \
    -a '  host' \
    -a '  git' \
    -a '  hg' \
    -a '  exec_time' \
    -a '  line_sep' \
    -a '  jobs' \
    -a '  exit_code' \
    -a '  char' \
    -a ')' \
    -a 'SPACESHIP_USER_SHOW=always' \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_CHAR_SYMBOL="❯"' \
    -a 'SPACESHIP_CHAR_SUFFIX=" "' \
    -p git \
    -p ssh-agent \
    -p  https://github.com/zsh-users/zsh-syntax-highlighting \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions

RUN add-apt-repository ppa:ondrej/php -y
RUN apt-get update
RUN apt-get install php8.1 -y
RUN apt-get install php8.1-bcmath -y
RUN apt-get install php8.1-xml -y
RUN apt-get install php8.1-fpm -y
RUN apt-get install php8.1-odbc -y
RUN apt-get install php8.1-pgsql -y
RUN apt-get install php8.1-mysql -y
RUN apt-get install php8.1-zip -y
RUN apt-get install php8.1-intl -y
RUN apt-get install php8.1-ldap -y
RUN apt-get install php8.1-gd -y
RUN apt-get install php8.1-cli -y
RUN apt-get install php8.1-bz2 -y
RUN apt-get install php8.1-curl -y
RUN apt-get install php8.1-mbstring -y
RUN apt-get install php8.1-opcache -y
RUN apt-get install php8.1-soap -y
RUN apt-get install php8.1-cgi -y

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

RUN sed -i "s|display_errors = Off|display_errors = On|g" /etc/php/8.1/apache2/php.ini

RUN usermod -a -G www-data root
RUN adduser root www-data
RUN chown -R www-data:www-data /var/www
RUN chmod -R g+rw /var/www
RUN chmod -R g+rws /var/www
RUN chgrp -R www-data /var/www/*
RUN chmod -R 1777 /var/www/html
EXPOSE 80:80
EXPOSE 8000:8000
WORKDIR /var/www/html
ENTRYPOINT service apache2 restart && zsh && bash && tail -f /dev/null