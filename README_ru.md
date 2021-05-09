# Jenkins X Docs
<a id="markdown-jenkins-x-docs" name="jenkins-x-docs"></a>

Этот репозиторий содержит исходный код для [jenkins-x.io](http://jenkins-x.io/)

**NOTE:** Пожалуйста, просматривайте эти документы на [jenkins-x.io](http://jenkins-x.io/). Не все ссылки работают при просмотре файлов Markdown внутри этого репохитория.

----

<!-- TOC -->

- [Создание документации](#building-the-docs)
    - [Подготовка исходников](#preparing-the-sources)
        - [Подмодули Git](#git-submodules)
    - [Загрузка модулей npm](#downloading-npm-modules)
    - [Запуск Hugo](#running-hugo)
        - [Локально](#locally)
        - [Внутри docker'а](#dockerized)
- [Common Workflows](#common-workflows)
    - [Запуск проверки орфографии](#running-spell-check)
    - [Проверка ссылок, изображений, и т.д.](#checking-links-images-etc)
    - [Добавление редиректов](#adding-redirects)
    - [Обновление контента `enhancements`](#upgrading-the-enhancements-content)
- [Localization](#localization)
- [Contributing](#contributing)

<!-- /TOC -->

----

## Создание документации
<a id="markdown-building-the-docs" name="building-the-docs"></a>

### Подготовка исходников
<a id="markdown-preparing-the-sources" name="preparing-the-sources"></a>

Для редактирования документов локально, вам необходимо склонировать этот репозиторий:

```bash
git clone  --recurse-submodules --depth 1 https://github.com/jenkins-x/jx-docs.git
```

#### Подмодули Git
<a id="markdown-git-submodules" name="git-submodules"></a>

Обратите внимание на использование `--recurse-submodules` в приведенной выше команде клонирования.
Эта опция подгружает два подмодуля git [submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), а именно  
[docsy](https://github.com/google/docsy) и [labs-enhancements](https://github.com/jenkins-x/enhancements).
Если у вас уже есть склонированный git репозиторий без подмодулей, вы можете запустить:

```bash
git submodule update --init --recursive
```

При последующий обновлениях исходников с помощью `git pull` также не забудьте получить изменения из подмодулей:

```bash
git pull --recurse-submodules
```

вы можете просмотреть `commit sha` подмодулей через:

```bash
git submodule status --recursive
```

### Загрузка модулей npm
<a id="markdown-downloading-npm-modules" name="downloading-npm-modules"></a>

После получения всех исходников вам необходимо [установить npm](https://www.npmjs.com/get-npm) и убедиться что необходимые модули npm установлены:

```bash
npm install
```

### Запуск Hugo
<a id="markdown-running-hugo" name="running-hugo"></a>

Сам сайт построен с помощью [Hugo](https://gohugo.io/) и настроен в [`config.toml`](./config.toml).
Вы имеете два варианта запуска Hugo, либо непосредственно на вашей локальной машине или используя [Docker Compose](https://github.com/docker/compose).
В следующих двух разделах эти варианты описаны более подробно.

**NOTE:** Hugo рендерит сайт в памяти в `development` режиме.
По умолчанию содержимое на диск не записывается.

#### Локально
<a id="markdown-locally" name="locally"></a>

Если вы хотите создать сайт локально на вашей машине, вам необходимо [установить](https://gohugo.io/getting-started/installing) Hugo.
Проверьте [Makefile](./Makefile) на предмет последней рекомендованной версии.
После установки, вы можете запустить:

```bash
make server
```

Теперь вы можете получить доступ к сайту по [localhost:1313](http://localhost:1313).

#### Внутри docker'а
<a id="markdown-dockerized" name="dockerized"></a>

Вместо установки Hugo локально, вы можете использовать предосталвенный `docker-compose.yml` для запуска сервера Hugo в контейнерной среде.
Убедитель что у вас установлен [Docker](https://docs.docker.com/install/).

```bash
make compose-up
```

Теперь вы можете получить доступ к сайту по адресу [localhost:1313](http://localhost:1313).
Сервер Hugo работает в фоновом режиме.
Вы можете остановить это с помощью:

```bash
make compose-down
```

## Common Workflows
<a id="markdown-common-workflows" name="common-workflows"></a>

### Запуск проверки орфографии
<a id="markdown-running-spell-check" name="running-spell-check"></a>

Не все мы мастера орфографии, поэтому, к счастью, есть инструменты, которые помогут нам это исправить.
Мы используем [node-markdown-spellcheck](https://github.com/lukeapage/node-markdown-spellcheck) что бы просмотреть все наши markdown файлы и перечислить любые орфографические ошибки или неизвестные слова.

Чтобы сделать это как можно проще, просто выполните следующую команду:

```bash
make spellcheck
```

Отчет, вероятно, включает слова, написанные правильно, но это просто означает, что средство проверки правописания не знает правильного написания (часто случается с техническими терминами, командами и т. Д.). 
Отредактируйте [`.spelling`](./.spelling) и добавьте неизвестное слово.
Кроме того, старайтесь, чтобы список был отсортирован в алфавитном порядке, чтобы упростить навигацию.

### Проверка ссылок, изображений, и т.д
<a id="markdown-checking-links-images-etc" name="checking-links-images-etc"></a>

Чтобы получить помощь в проверке всех ссылок, мы используем [htmlproofer](https://github.com/chabad360/htmlproofer).

```bash
make linkcheck
```

**NOTE:** Первоначальный запуск выполняется очень медленно (из-за проверок внешних ссылок), и кэш накапливается только после его завершения.

**NOTE:**: На данный момент можно безопасно игнорировать ошибки `... x509: certificate ...`

### Добавление редиректов
<a id="markdown-adding-redirects" name="adding-redirects"></a>

Если вы переместите страницу в другое место, вы можете добавить перенаправление с помощью записи _aliases_ в заголовке страницы:

```yaml
aliases:
  - /some/old/path
  - /another/path
```  

### Обновление контента `enhancements`
<a id="markdown-upgrading-the-enhancements-content" name="upgrading-the-enhancements-content"></a>

Для обновления новых коммитов в `enhancements` - мы, надеюсь скорость автоматизируем это!

```bash
cd content/en/docs/labs/enhancements
git checkout master
git pull
cd ..
git add enhancements
git commit -m "move to latest enhancements"
```

## Localization
<a id="markdown-localization" name="localization"></a>

Что бы больше людей знали Jenkins X лучше, необходима локализация. 
И мы должны соблюдать некоторые правила по этому поводу, пожалуйста, прочтите правила для соответствующих языков ниже:

- [Chinese](Localization_Chinese.md)

## Contributing
<a id="markdown-contributing" name="contributing"></a>

Руководство по созданию документации доступно на [Jenkins X website](https://jenkins-x.io/ru/community/documentation/).
