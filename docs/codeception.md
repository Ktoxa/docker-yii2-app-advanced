
##Codeception docker image 

see [Parallel Execution](http://codeception.com/docs/12-ParallelExecution)


Development bash
```
docker-codeception-run$ docker-compose run --rm --entrypoint bash codecept
```

[How to start testing](https://github.com/yiisoft/yii2-app-advanced/blob/master/docs/guide/start-testing.md)




###notes
`Codeception` has been changed:

Use source `codeception/codeception`

Base image change: `codeception/codeception` -> `bscheshir/php:7.1.0-fpm-4yii2-xdebug`

how to create it:
```
cd docker-codeception-run
git submodule add https://github.com/Codeception/Codeception.git docker-codeception-run/build
cd docker-codeception-run/build
git checkout 2.2 
cp ../Dockerfile ../composer.json ./ 
docker build -t bscheshir/codeception:php7.1.0-fpm-4yii2 .
```

Dockerfile: based on php7 for Yii2 docker image
```
sed -i -e "s/^FROM.*/FROM bscheshir\/php:7.1.0-fpm-4yii2/" Dockerfile
```

require `codeception/specify`, `codeception/verify`
`composer.json`
```
    "require": {
...
        "codeception/specify": "*",
        "codeception/verify": "*"
    }
```

Composition volumes `tests`:
```
  codecept:
    image: bscheshir/codeception:7.0.13-fpm-4yii2
    depends_on:
      - php
    environment:
      XDEBUG_CONFIG: "remote_host=192.168.0.241 remote_port=9002 remote_enable=On"
      PHP_IDE_CONFIG: "serverName=codeception"
    volumes:
      - ../php-code/tests:/project
```

For smart IDE copy source to `test/.repo` 
```
root@e870b32bc227:/project# cp -r /repo/ /project/tests/.repo
```