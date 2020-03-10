FROM bscheshir/codeception:php7.4.3-fpm-alpine-yii2
# note: this Dockerfile will be used for autotest reason only. We can use RUN on each line as marker of potentially error breakpoint.
COPY ./run_tests.sh /run_tests.sh
RUN /bin/bash -c 'chmod +x /run_tests.sh'
RUN cat /run_tests.sh
RUN ls -luha /run_tests.sh
RUN curl -L https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh -o /usr/local/bin/wait-for-it.sh
RUN /bin/bash -c 'chmod +x /usr/local/bin/wait-for-it.sh'
RUN ls -luha /usr/local/bin/wait-for-it.sh
ENTRYPOINT ["/usr/local/bin/wait-for-it.sh", "db:3306", "--", "/run_tests.sh"]
RUN git clone https://github.com/yiisoft/yii2-app-basic.git /var/www/html
RUN composer install --no-dev
# replace some config same to docker-compose.test.yml
RUN sed -i -e "s/.*dsn.*/"\'"dsn"\'" => "\'"mysql:host=db;dbname=yii2basic"\'",/" -e "s/.*username.*/"\'"username"\'" => "\'"yii2basic"\'",/" -e "s/.*password.*/"\'"password"\'" => "\'"yii2basic"\'",/" -e "s/.*charset.*/"\'"charset"\'" => "\'"utf8mb4"\'",/" /var/www/html/config/db.php
RUN sed -i -e "s/test_db.php/db.php/" /var/www/html/config/test.php
