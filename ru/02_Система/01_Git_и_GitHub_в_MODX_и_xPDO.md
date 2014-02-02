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

### Community Contributor's Guide
With that basis in the workflow, your next step is to read the Community Contributor's Guide to understand the branching model MODX is using and for more detail on putting it into practice.


A GitHub-based branching strategy for collaborative development

In order to facilitate collaborative development on the MODX source code managed at GitHub, a clear and consistent branching strategy has been adopted. This strategy consists of maintaining two permanent branches in each main Git repository: master, which represents code that is assumed to be in a production-ready state, and develop, which contains work to be incorporated into the "next release". However, there are a number of important supporting branches that will only live for a limited amount of time, including feature branches, production hotfix branches, and specific release branches. Though they are normal Git branches, they differ significantly in the way they are used in the development process.
The permanent branches

The master branch should be familiar to any Git user, representing the stable, production-ready code in the repository. In our process, we maintain another branch with an infinite lifetime, develop. You can think of this as the "integration branch" where all changes are delivered for the next release. This is also where nightly builds will originate.

When the code in develop reaches a stable point and is ready to be released, all of the changes will be merged back to the master branch and tagged with a release number. Each merge commit back to master represents a production release, by definition.
Supporting branches

There are a number of supporting branches in our process that are used to aid in collaborative development of bugfixes, translation updates, features, preparing releases, and quickly applying patches to production releases. These branches are referred to as:

    Feature branches - these are the branches that you will be working with as a community contributor
    Release branches
    Hotfix branches

Each has a special purpose and strict rules governing origination and merge targets, but are otherwise normal Git branches.
Working with your GitHub fork

MODx contributors must work directly with their private forks on GitHub. Here is the suggested way to prepare your local repository as a developer for contributing back to any MODx project:

$ git clone git@github.com:YourGitUsername/revolution.git
$ cd revolution
$ git remote add upstream -f http://github.com/modxcms/revolution.git

This setup makes your fork the standard origin remote, and adds/fetches the "blessed" repository as the remote upstream. You may want to add other remotes to other developer forks as well, and I would name those remotes appropriately so you can keep track of each one.

You'll want to go ahead and create local tracking branches for the permanent branches from your fork, a.k.a. origin:

$ git checkout -b master origin/master
Switched to a new branch "master"
$ git checkout -b develop origin/develop
Switched to a new branch "develop"

To keep your local tracking branches for develop and master up-to-date from the upstream repository:

$ git fetch upstream
$ git checkout develop
Switched to branch "develop"
$ git merge --ff-only upstream/develop
$ git checkout master
Switched to branch "master"
$ git merge --ff-only upstream/master
$ git push origin develop master

Note however, that the push is mainly for show, as the permanent branches should never be a target for contributor commits, even in the forks. IOW, develop and master in your fork should always match the upstream branches of the same name. It is expected that all contributions will be submitted via a feature or hotfix branch originating from the appropriate permanent branch, or a bug fix branch originating from a release branch in the upstream repository.

Also note the --ff-only flag ensures that only fast-forward merges are performed (in case you accidentally do commit to the main branches on your fork without realizing it).
Important
Please make sure you have your autocrlf settings set appropriately before making any commits to your fork. See http://help.github.com/dealing-with-lineendings/ to determine the setting you need based on the platform you are developing on.
Feature branches

    May branch from: develop
    Naming convention: anything except master, develop, release-, or <strong>hotfix-</strong>

Feature branches, also known as topic branches, are used to develop a specific new feature (or set of features) for the next release, or for a future release. The target release for the feature to be incorporated may well be unknown, and the branch will exist as long as that feature is in development. Once it is accepted and ready to be incorporated in the next release, it is merged into the develop branch by an integrator. If the feature is never completed or accepted, it can simply be discarded.

Feature branches typically exist in developer forks, and only for sharing purposes, not in the "blessed", or upstream repository.
Creating a feature branch

When starting work on a new feature, branch off from the develop branch.

$ git checkout -b myfeature develop
Switched to a new branch "myfeature"

Submitting a pull request for a finished feature

Once you have completed development of a feature on a branch, you should first make sure your work is replayed over the latest updates from develop:

$ git fetch upstream
$ git checkout develop
Switched to branch {{develop}}
$ git merge --ff-only upstream/develop
$ git checkout myfeature
Switched to branch "myfeature"
$ git rebase develop

This will make it easier for integrators to incorporate your work without conflict.

Now simply push your feature to your fork (you can do this early on if you want to share your feature branch for collaboration):

$ git push origin myfeature:myfeature

And you are ready to submit a pull request for your feature branch.
Bug Branches

If there's a bug in the MODX Bug Tracker that you would like to fix, here's a simple workflow you can follow.

First, fork the MODX Git repo on github, then clone your fork (see above).

You may wish to start clean if you already have a release branch locally. E.g. if you already have a "release-2.2" branch, you can delete it locally and start clean:

git branch -D release-2.2

Next, you'll want to checkout the branch fresh from upstream:

git fetch upstream
git checkout -b release-2.2 upstream/release-2.2

Before you begin work on coding your fix, create a branch devoted to your upstream target (where XXXX is the bug number):


git checkout -b bug-XXXX release-2.2

Now you're ready to do your changes. Fix the bug!

Once the bug is fixed, you can commit your changes and push your bugfix branch to your fork:

git commit .
git push origin bug-XXXX

Then you're ready to issue your pull request from Github.

Log into your Github account, find your MODX fork, then hit the button at the top that says "Pull Request".

Make sure you select the "base branch" – you want to issue the pull request to the branch that initially checked out.

Git FAC (Frequently Accessed Commands)

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

[1]: http://git-scm.com/
[2]: http://ru.wikipedia.org/wiki/Система_управления_версиями
[3]: http://habrahabr.ru/post/68341/
[4]: http://www.ndpsoftware.com/git-cheatsheet.html
[5]: http://try.github.io
[6]: https://github.com/features/hosting
[7]: http://gitref.org/
[8]: https://help.github.com/articles/fork-a-repo
[9]: https://help.github.com/articles/using-pull-requests