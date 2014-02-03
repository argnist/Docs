**Git** используется в качестве системы контроля версий, менеджер задач и баг-трекер для MODX. Как и MODX, он это свободен и с открытым исходным кодом.

* [Официальный сайт Git'а][1] прекрасное место, с которого вы можете начать изучать Git или принципы [контроля версий][2].
Там вы узнаете как установить Git и познакомитесь с его базовыми настройками и командами.
* Если вы знакомы с SVN, то вам может быть полезна эта [статья о переходе с SVN на Git][3].
* [Интерактивная шпаргалка по Git][4]
* [Интерактивный курс по Git][5]

**GitHub** это хостинг для репозиториев Git, в том числе и репозитория MODX. **GitHub** это не только сервис для "безопасного
хранения исходного кода и совместной разработки", но и социальная сеть для программистов.

Подробнее о [GitHub][6] и [справочник команд Git (англ.яз.)][7].

## Обзор

Здесь описывается технологический процесс, которого следует придерживаться, внося поправки в любой репозиторий MODX. Он
поможет понять основы перед тем, как погрузиться более детально.

#### Форк (Fork)
Сначала вы создаете форк репозитория в своем собственном аккаунте GitHub. Это "ваш форк". Вы будете публиковать свои
изменения в коде (коммиты) в ваш форк, не напрямую в *modxcms* репозиторий (или какой-либо другой). Это делает Git
*распределенным*, в то время как SVN централизован вокруг одного репозитория.

Затем для работы с форком вам понадобится локальная копия, т.е. клон вашего репозитория.

[Руководство GitHub по созданию форка репозитория и его локальной копии][8].

#### Создание ветви и написание кода

Вся работа над отдельной задачей (ошибкой или новой функцией) должна вестись в отдельной ветке под эту задачу.
```
$ git checkout -b bug-1111
```
Эта команда означает, что создается новая ветка *bug-1111*, и мы переключаемся на нее.
Затем вы изменяете файл или несколько. Вы можете сделать один или несколько коммитов в этой ветке (множество коммитов могут
помочь поддерживать организованность проекта в определенных обстоятельствах).

По ходу дела или, когда работа сделана, вы отправляете (push) ветку в свой форк (вы увидите новую ветку и свои коммиты на GitHub) следующей командой:
```
git push myRepo bug-1111
```

> Замечание:

> Убедитесь, что ваша работа и коммиты основаны на свежем коде, чтобы избежать проблем в интеграции вашего кода в основной проект.

#### Pull Request

Когда вы готовы внести изменения из своей ветки в основной репозиторий, вам следует сделать [Pull Request][9] из вашего аккаунта GitHub.

Ваш Pull Request может быть принят как есть или, ответственный за интеграцию может сделать некоторые изменения или прокомментировать, задать вопрос и т.п.
GitHub способствует общению с помощью как комментариев на отдельные строки, так и простых форумов в Пул Реквестах.

## Руководство по совместной разработке сообществом MODX

После знакомства с основами следующим шагом будет подробнее на практике разобраться в модели ветвления MODX.

### Основанная на GitHub стратегия ветвления для совместной разработки

Чтобы облегчить совместную разработку над исходным кодом MODX, содержащимся на GitHub, принята понятная и последовательная стратегия ветвления.
Эта стратегия состоит из поддержки двух постоянных ветвей в каждом Git репозитории: *master*, которая содержит код стабильной версии, и *develop*,
которая содержит все наработки для следующего релиза. Тем не менее существуют и другие важные ветки, которые поддерживаются некоторое время,
включая ветки новых функций, заплатки для стабильных версий и ветки отдельных релизов. Хотя они и такие же ветки Git, способ,
которым они используются в процессе разработки, значительно отличается.

#### Постоянные ветки

Ветка *master* должна быть знакома каждому пользователю Git. Она содержит стабильный, готовый к установке на "боевом" сервере код.
Другая постоянная ветка *develop* представляет собой интеграционную ветку, где содержатся все изменения для последующего релиза.
Так же из этой ветки собираются "ночные билды" (nightly builds).

Когда код в ветке *develop* достигает стабильного состояния и готов к релизу, все изменения сливаются с веткой *master* и помечаются релизным номером.
Каждый коммит слияния (merge commit) в *master* представляет собой релиз по определению.

#### Ветки поддержки

Так же есть несколько поддерживающих веток, которые используются с целью совместной разработки багфиксов, переводов, новых функций, подготовки к релизам
и быстрого применения патчей к готовым релизам.

### Работа с вашим форком GitHub

MODx разработчики должны работать напрямую только со своими форками на GitHub. Здесь предлагается способ подготовки вашего локального репозитория
к разработке любого проекта MODX (в том числе так же вы можете работать и над данной документацией):
```
$ git clone git@github.com:YourGitUsername/revolution.git
$ cd revolution
$ git remote add upstream -f http://github.com/modxcms/revolution.git
```
Эти команды клонируют локально ваш стандартный форк на GitHub (*origin*) и добавляет основной удаленный репозиторий *upstream*.
Вы можете добавить и другие удаленные форки, чтобы отслеживать изменения и на них.

Вы можете захотеть создать локальные следящие ветки для постоянных веток из вашего форка, или *origin*:
```
$ git checkout -b master origin/master
Switched to a new branch "master"
$ git checkout -b develop origin/develop
Switched to a new branch "develop"
```

Чтобы содержать обновленными свои локальные следящие ветки для *develop* и *master* из репозитория *upstream*:
```
$ git fetch upstream
$ git checkout develop
Switched to branch "develop"
$ git merge --ff-only upstream/develop
$ git checkout master
Switched to branch "master"
$ git merge --ff-only upstream/master
```

> Замечание:

> Постоянные ветки никогда не должны быть прямой целью коммитов, даже в форках. Другими словами, *develop* и *master* в вашем форке всегда должны совпадать
с *upstream* ветками с тем же названием. Предполагается, что весь новый код будет добавляться через ветки *feature* или *hotfix*,
создаваемых из подходящей постоянной ветки *upstream репозитория*.

Также заметьте, что флаг **--ff-only** гарантирует, что выполнятся только *fast-forward merges* (в случае если вы случайно сделали коммит в основные ветки вашего форка).

> Важно

> Удостоверьтесь, что ваща настройка **autocrlf** установленна правильно перед коммитами в ваш форк. Подробнее о [настройке Git][10].

### Ветки новых функций

* Могут ответвляться от: *develop*
* Могут именоваться: как угодно за исключением *master*, *develop*, *release-*, или ***hotfix***

Ветки новых функций или тематические ветки используются для разработки конкретных новых функций (или множества функций) для следующего или будущего релиза.
Целевой релиз может быть неизвестен, и ветка будет существовать так долго, сколько потребуется на разработку данной новой функции.
Как только она принята и готова к внедрению в следующем релизе, ветка сливается интегратором в ветку *develop*.
Если новая особенность долго не завершается или принимается, она может быть просто удалена.

Такие ветки обычно содержатся в форках разработчиков и только в целях совместного использования, не в *upstream* репозитории.

### Создание тематической ветки

Когда начинаете работать над новой функцией, сделайте ответвление от ветки *develop*:
```
$ git checkout -b myfeature develop
Switched to a new branch "myfeature"
```

### Отправка пулл реквеста с законченной функцией

Как только вы завершили разработку новой функции в отдельной ветке, следует сначала удостовериться, что ваша работа воспроизводится с последними изменениями из ветки *develop*:
```
$ git fetch upstream
$ git checkout develop
Switched to branch {{develop}}
$ git merge --ff-only upstream/develop
$ git checkout myfeature
Switched to branch "myfeature"
$ git rebase develop
```

Это поможет интеграторам легче внедрить вашу работу без конфликтов.

Теперь просто отошлите (push) ваши изменения в свой форк (вы можете сразу это сделать, если хотите поделиться своей веткой разработки с другими):
```
$ git push origin myfeature:myfeature
```

И вы готовы подать [pull request][11] вашей тематической ветки.

### Ветки исправления ошибок

Если [есть ошибка в MODX][12], которую бы вы хотели исправить, следуйте следующему простому процессу.

Для начала создайте форка Git репозитория MODX в своем GitHub аккаунте, затем клонируйте его на локальный компьютер (как было описано ранее).

Вы можете начать локальную релизную ветку заново. Например, если у вас уже есть ветка "*release-2.2*", вы можете удалить ее локально и начать заново:
```
git branch -D release-2.2
```
Затем вам следует скачать свежую ветку из *upstream*:
```
git fetch upstream
git checkout -b release-2.2 upstream/release-2.2
```

Перед тем, как вы начнете работать над кодом, создайте ветку, посвященную вашей *upstream* цели (где **XXXX** это номер ошибки):
```
git checkout -b bug-XXXX release-2.2
```

Теперь вы готовы делать свои изменения. Исправьте ошибку!

Как только ошибка исправлена, вы можете сделать коммит своих изменений и отправить свою ветку исправления ошибки в свой форк:
```
git commit .
git push origin bug-XXXX
```

Затем вы готовы выпустить свой pull request на Github.

Войдите в свой аккаунт Github, найдите свой форк MODX, затем нажмите кнопку сверху с надписью "Pull Request".
[![](http://rtfm.modx.com/download/attachments/33948128/github_modx_pull_request.jpg)](http://rtfm.modx.com/download/attachments/33948128/github_modx_pull_request.jpg)

Проверьте, что выбрали "базовую ветку" – ту, от которой вы ранее ответвляли ветку для исправления ошибки.

## Git FAC (Frequently Accessed Commands)

How do I get and keep my local develop branch in sync?

First, with MODX's collaboration and branching model, you won't be making commits to your develop branch, so it's easy to keep it up to date.

$ git fetch upstream
$ git checkout develop
Switched to branch "develop"
$ git merge --ff-only upstream/develop

This assumes that the modxcms or blessed repo is set up as a remote named upstream. (git remote manpage: http://www.kernel.org/pub/software/scm/git/docs/git-remote.html)
How do I create a feature branch?

If you've just merged in the upstream repo's develop branch, then it's simple:

$ git checkout develop
Switched to branch "develop"
$ git checkout -b myFeatureBranchName

Is there a naming convention for feature branches?

If you are fixing a bug from a ticket in the issue tracker (please file a ticket for the bug first if there isn't one) then you can name your branch "bug-xxxx" where xxxx is the issue number from the bug tracker.

$ git checkout -b bug-1234

If you are working on an "improvement" ticket in the issue tracker, use the prefix "impr-"

$ git checkout -b impr-2345

If you are working on a feature which does not have a ticket, you can name it anything except master, develop, release-, or hotfix-

$ git checkout -b myAwesomeFeature

Do I need a new feature branch for every issue that I work on?

Yes.

$ echo 'Yes'

How do I keep my feature branch in sync with the upstream develop branch?

If you're working on a feature that's taking a while, you may find it beneficial to keep in sync with upstream changes. Git allows you to "replay" your commits over top of changes in the develop branch using the rebase command.

In fact, it's generally a good idea to do this before any final commits to your fork and issuing a Pull Request.

$ git fetch upstream
$ git checkout develop
Switched to branch {{develop}}
$ git merge --ff-only upstream/develop
$ git checkout myfeature
Switched to branch "myfeature"
$ git rebase develop

To learn more, here's the git rebase manpage: http://www.kernel.org/pub/software/scm/git/docs/git-rebase.html
Do I really need to worry about the master branch?

No, not really. If you want to, you can. But it's not critical to the workflow of a contributor at all.

$

Contributing to xPDO and Integrating with MODX Revolution

xPDO is an object-oriented framework on which MODX Revolution is built. It is maintained in a separate git repository from revolution and contributing to the xPDO core of MODX requires some additional work.

xPDO contributors should follow the same basic process and branching strategy as those for MODX itself. See the MODx GitHub Contributor's Guide for details on the branching strategies for features, releases, hotfixes, and more. This guide will focus on the additional steps required to integrate your xPDO changes into MODX for testing before submitting Pull Requests on the xPDO repository.
Forking and Cloning the Complete xPDO Repository

As with MODX, this means contributors must work directly with their private forks on GitHub. Here is the suggested way to prepare your local repository as a developer for contributing back to the complete xPDO project:

[repos]$ git clone git@github.com:YourGitUsername/xpdo.git
[repos]$ cd xpdo
[xpdo]$ git remote add upstream -f http://github.com/modxcms/xpdo.git

This setup makes your fork the standard origin remote, and adds/fetches the "blessed" repository as the remote upstream. You may want to add other remotes to other developer forks as well, and I would name those remotes appropriately so you can keep track of each one.

You'll want to go ahead and create local tracking branches for the permanent branches from your fork, a.k.a. origin:

[xpdo]$ git checkout -b master origin/master
Switched to a new branch "master"
[xpdo]$ git checkout -b develop origin/develop
Switched to a new branch "develop"

To keep your local tracking branches for develop and master up-to-date from the upstream repository:

[xpdo]$ git fetch upstream
[xpdo]$ git checkout develop
Switched to branch "develop"
[xpdo]$ git merge --ff-only upstream/develop
[xpdo]$ git checkout master
Switched to branch "master"
[xpdo]$ git merge --ff-only upstream/master
[xpdo]$ git push origin develop master

Note however, that the push is mainly for show, as the permanent branches should never be a target for contributor commits, even in the forks. IOW, develop and master in your fork should always match the upstream branches of the same name. It is expected that all contributions will be submitted via a feature or hotfix branch originating from the appropriate permanent branch, or a bug fix branch originating from a release branch in the upstream repository.

Also note the --ff-only flag ensures that only fast-forward merges are performed (in case you accidentally do commit to the main branches on your fork without realizing it).
Important
Please make sure you have your autocrlf settings set appropriately before making any commits to your fork. See http://help.github.com/dealing-with-lineendings/ to determine the setting you need based on the platform you are developing on.
Unit Tests
xPDO has a growing number of Unit Tests which help ensure at least basic functionality is not broken when changes are made to the code base. Make sure your changes pass the unit tests for ALL implemented drivers when submitting any Pull Requests to, or that affect, xPDO code. In addition, all bug fixes and features should be accompanied with new unit test cases where practical.
Forking and Cloning the xPDO Core Repository

xPDO has two GitHub repositories. The complete repository contains Unit Tests, test models, build scripts and other assets that you would not want integrated into other projects. In order to more easily merge the project within other projects, a second repository that includes only the xpdo/ subdirectory (this contains the run-time files of xPDO only) was created. This repository is kept in sync with the complete xPDO repository via git's subtree merge technique, and this same technique can then be used to merge the xPDO Core with any other git repository.

So the next step is to fork and clone this repository as well:

[repos]$ git clone git@github.com:YourGitUsername/xpdo-core.git
[repos]$ cd xpdo-core
[xpdo-core]$ git remote add upstream -f http://github.com/modxcms/xpdo-core.git

Migrating Changes for Testing

Whenever you have completed a feature or bugfix in the complete xPDO repository, all the unit tests are passing, and you are ready to test the change in another project, you can push the change to an appropriate branch on your fork. Once there, you can choose to manually copy your modified files into place in any external project you are using xPDO with, or use git's subtree merge technique to update the xpdo-core repository with your changes, and then turn around and do the same from xpdo-core into your project's repository.

To update the xpdo-core repository, we first need to add and fetch a remote for your fork of the complete xpdo repository:

[xpdo-core]$ git remote add -f xpdo git@github.com:YourGitUsername/xpdo.git

Once added, you can fetch changes you commit to your xpdo fork and merge them easily. Make sure your xpdo-core branches are up-to-date from upstream first, e.g. if pushing a feature branch called xpdo/feature-1234 off of the upstream/develop branch:

[xpdo-core]$ git fetch upstream
[xpdo-core]$ git checkout develop
Switched to branch "develop"
[xpdo-core]$ git merge --ff-only upstream/develop
[xpdo-core]$ git push origin develop
[xpdo-core]$ git fetch xpdo
[xpdo-core]$ git checkout -b feature-1234 develop
[xpdo-core]$ git merge -s subtree --log xpdo/feature-1234
[xpdo-core]$ git push origin feature-1234

At this point, your feature branch is in your xpdo-core fork and ready for merging into MODX Revolution or any other project that has xpdo-core subtree merged into it.
Testing Changes in MODX Revolution

Now, to test your changes with MODX Revolution, you need to add and fetch your fork of the xpdo-core repository as a remote to your revolution fork. Once you do that, you can create a branch to merge in and test your xpdo-core feature branch. Change directory to your MODX Revolution git fork and get it up-to-date.

[revolution]$ git checkout develop
Switched to branch "develop"
[revolution]$ git fetch upstream
[revolution]$ git merge --ff-only upstream/develop
[revolution]$ git push origin develop
[revolution]$ git remote add -f xpdo git@github.com:YourGitUsername/xpdo-core.git
[revolution]$ git checkout -b xpdo-feature-1234 develop
[revolution]$ git merge -s subtree --log xpdo/feature-1234

If it works, share your branch with the xpdo-core changes integrated with the world to play with:

[revolution]$ git push origin xpdo-feature-1234

Submitting the Pull Request

After all of this is done, and you are confident your changes should make it into xPDO, all you need to do is submit your original feature branch, on your fork of the complete xpdo repository, to the appropriate branch of the upstream xpdo repository.

[1]: http://git-scm.com/book/ru
[2]: http://ru.wikipedia.org/wiki/Система_управления_версиями
[3]: http://habrahabr.ru/post/68341/
[4]: http://www.ndpsoftware.com/git-cheatsheet.html
[5]: http://try.github.io
[6]: https://github.com/features/hosting
[7]: http://gitref.org/
[8]: https://help.github.com/articles/fork-a-repo
[9]: https://help.github.com/articles/using-pull-requests
[10]: http://git-scm.com/book/ru/Настройка-Git-Конфигурирование-Git#Форматирование-и-пробельные-символы
[11]: http://help.github.com/pull-requests/
[12]: https://github.com/modxcms/revolution/issues