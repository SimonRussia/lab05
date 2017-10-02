[![Build Status](https://travis-ci.org/SimonRussia/lab05.svg?branch=master)](https://travis-ci.org/SimonRussia/lab05)
## Laboratory work V

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```ShellSession
$ open https://travis-ci.org
```

## Tasks

- [X] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [X] 2. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [X] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [X] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [X] 7. Установить [**Travis CLI**](https://github.com/travis-ci/travis.rb#installation)
- [X] 8. Выполнить инструкцию учебного материала
- [X] 9. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

Определяем глобальные переменные (**ID пользователя**).
```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_TOKEN=<полученный_токен>
```

Подготовка к выполнению **Лабораторной работы №5**.
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 lab05
$ cd lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
```

Создаем конфигурационный файл для среды **Travis CI**.
```ShellSession
$ cat > .travis.yml <<EOF
language: cpp

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```

Подключаем **Travis CI** к аккаунту на **GitHub**.
```ShellSession
$ travis login --github-token ${GITHUB_TOKEN}
```

Проверяем созданный конфигурационный файл `.travis.yml`.
```ShellSession
$ travis lint
```

Добавляем в файл **README.md** строку с **markdown**-кодом индикатора репозитория на **Travis CI**.
```ShellSession
$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
```

Выгружаем созданный `.travis.yml` на **GitHub** сервис.
```ShellSession
$ git add .travis.yml
$ git add README.md
$ git commit -m"added CI"
$ git push origin master
```

Тестируем комманды **Travis CI**.
```ShellSession
$ #Проверяет .travis.yml файл на корректность.
$ travis lint
$ #Проверяет состояние подключения сервиса Travis CI к аккаунту(-ам) на сервисе GitHub.
$ travis accounts
$ #Синхронизирует данные с сервисом GitHub. (Можно выполнить это действие на странице профиля).
$ travis sync
$ #Выводит список созданных в подключенном аккаунте на GitHub репозиториев и их параметры (в частности их подключение к среде Travis CI, а также парава пользователя на доступ к ним).
$ travis repos
$ #Показывает доступные проекты.
$ travis enable
$ #Выводит список последних сборок.
$ travis whatsup
$ #Отображает последние сборки для всех веток проекта.
$ travis branches
$ #Отображает историю работы (действий) с проектом.
$ travis history
$ #Выводит данные о сборке и состояние проекта.
$ travis show
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Travis Client](https://github.com/travis-ci/travis.rb)
- [AppVeyour](https://www.appveyor.com/)
- [GitLab CI](https://about.gitlab.com/gitlab-ci/)

```
Copyright (c) 2017 Братья Вершинины
```
