---
title: Conditional include in git config
layout: post
tags: [git, development]
---
Git [обновился](https://github.com/blog/2360-git-2-13-has-been-released). Добавилась очень интересная фича: [conditional includes](https://git-scm.com/docs/git-config#_includes) для конфигов. Самый простой пример, зачем это нужно, это разные настройки пользователя (имя и почта) для разных проектов. Можно рабочие репозитории держать в `~/work`, а остальное в `~/pleasure` (зачем в такой директории держать репозитории не очень ясно, но для примера это нужно). И для разных директорий подключать разные конфиги.

Сначала я подумал, что это бред. Ну нафига иметь на 99% одинаковые конфиги, отличающиеся только пользователем? Ведь нет же смысла иметь разные алиасы или другие настройки. Неправда моя. Смысл есть. И работает все немножко не так.

Подключаемый файл раскрывается сразу в том месте, где его подключили. Что будет, если в конфигах есть одинаковые блоки с разным определением одинаковых алиасов? Самое время пересобрать гит и проверить!

```sh
[master ✓] $ git --version
git version 2.13.0
```

Имеем следующую конфигурацию:

```sh
[master ✓] $ ls -a ~ | grep gitconfig
.gitconfig
.gitconfig-test

[master ✓] $ cat ~/.gitconfig-test
[alias]
    st = status

[master ✓] $ cat ~/.gitconfig
    ...
    [includeIf "gitdir:~/"]
        path = ~/.gitconfig-test
    [alias]
        st = status -sb
    ...
```
Важно, что подключаемый конфиг стоит до блока с алиасами. В этом случае срабатывает алиас из главного конфига. Если же блок `[includeIf "gitdir:~/"]` поставить после блока алиасов, то отработает подключаемый конфиг. При этом алиасы из главного конфига тоже сохраняются. Это все говорит о том, что работает новая фича очень логично и правильно.

Теперь о том, как это можно использовать в реальной жизни.

Содержимое `.gitconfig` (сокращено для удобства):

```ini
[user]
    name = Maslennikov, Andrey
    email = andrew.maslennikov@gmail.com
[alias]
    st = status -sb
[includeIf "gitdir:~/work"]
    path = ~/.gitconfig-work
[includeIf "gitdir:/"]
    path = ~/.gitconfig-machine
```

Содержимое `.gitconfig-work` (подключается только для репозиториев в специальной директории):

```ini
[user]
    name = Maslennikov, Andrey
    email = andrew.maslennikov@<company>.com
```

Содержимое `.gitconfig-machine` (подключается всегда на этой машине):

```ini
[http]
    proxy = http://user:pass@proxy.at.your.org:port
```

Что это дает:

 - главный конфиг работает всегда и везде без изменений: и на домашней, и на рабочей машинах.
 - `work` конфиг работает для рабочих проектов (будьте [осторожны](https://www.joelonsoftware.com/2016/12/09/developers-side-projects/) с разработкой личных проектов на рабочей машине)
 - `machine` конфиг позволяет настроить то, что важно только для этой машины. Это могут быть прокси или настройки инструментов для `diff`/`merge` операций.
 - подключаемых файлов может и не существовать, если верить [этому тесту](https://github.com/git/git/blob/3efd0bedc6625a6b194c1f6e5f1b7aa7d8b7e6bb/t/t1305-config-include.sh#L103)

Остается только обновить гит на всех машинах и перейти на эту замечательную схему.
