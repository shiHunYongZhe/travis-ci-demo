```
实验目的
了解持续集成的做法，学会使用 Travis CI。
操作步骤
（1）注册 Github 的账户。如果你已经注册过，跳过这一步。

（2）访问这个代码库github.com/ruanyf/travis-ci-demo，点击右上角的Fork按钮，将它克隆到你自己的空间里面。

（3）将你fork的代码库，克隆到本地。注意，要将下面网址之中的[your_username]改成你的 Github 用户名。

// Linux & Mac
$ git clone git@github.com:[your_username]/travis-ci-demo.git

// Windows
$ git clone https://github.com:[your_username]/travis-ci-demo
（4）使用你的 Github 账户，登录 Travis CI 的首页。然后，访问 Profile 页面，选定travis-ci-demo代码库运行自动构建。

（5）回到命令行，进入你本地的travis-ci-demo目录，切换到demo01分支。

$ cd travis-ci-demo
$ git checkout demo01
项目根目录下面有一个.travis.yml文件，这是 Travis CI 的配置文件。如果没有这个文件，就不会触发 Travis CI 的自动构建。打开看一下。

language: node_js
node_js:
  - "node"
上面代码指定，使用 Node 完成构建，版本是最新的稳定版。

指定 Node 的版本号也是可以的。

language: node_js
node_js:
  - "4.1"
上面代码指定使用 Node 4.1 版。

（6）Travis CI 默认依次执行以下九个脚本。

before_install
install
before_script
script
after_success 或者 after_failure
after_script
before_deploy（可选）
deploy（可选）
after_deploy（可选）
用户需要用到哪个脚本，就需要提供该脚本的内容。

对于 Node 项目，以下两个脚本有默认值，可以不用自己设定。

"install": "npm install",
"script": "npm test"
（7）打开当前分支的package.json，可以发现它的test脚本是一个lint命令。

"scripts": {
  "test": "jshint hello.js"
},
（8）在项目根目录下，新建一个新文件NewUser.txt，内容是你的用户名。提交这个文件，就会触发 Travis CI 的自动构建。

$ git add -A
$ git commit -m 'Testing Travis CI'
$ git push
（9）等到 Travis CI 完成自动构建，到页面上检查构建结果。

（10）切换到demo02分支，打开package.json，可以看到test脚本，现在需要完成两步操作了。

  "scripts": {
    "lint": "jshint hello.js hello.test.js",
    "test": "npm run lint && mocha hello.test.js"
  },
（11）重复上面第 8 步和第 9 步。

练习
修改hello.js，让其输出Hello Node。并修改测试用例hello.test.js，使之能够通过 Travis CI 的自动构建。

下面的英文说明是原作者的，我没看
```



This is a beginner tutorial of Travis CI for Node projects.

## How to use


**Step 1**

Fork the repo (If you don't know what is fork, [click here](https://guides.github.com/activities/forking/)). Then, clone your fork into disk.

```bash
$ git clone git@github.com:[your_username]/travis-ci-demo.git
```

**Step 2**

Sign in to [Travis CI](https://travis-ci.org/auth) with your GitHub account. Go to [profile page](https://travis-ci.org/profile) and open the travis-ci-demo repository to run Travis CI builds.

**Step 3**

Return to your termial window. Change into the travis-ci-demo directory, and switch into the `demo01` branch.

```bash
$ cd travis-ci-demo
$ git checkout demo01
```

Create an empty `NewUser.txt` file. Add the file to git, commit and push, to trigger a Travis CI build.

```bash
$ touch NewUser.txt
$ git add -A
$ git commit -m 'Testing Travis CI'
$ git push
```

**Step 4**

Go to [Travis CI](https://travis-ci.org/). Wait for it to run a build on your repository, check the [build status](https://travis-ci.org/repositories). (Travis CI will sends an email to tell you the build result as well.)

**Step 5**

Switch into other demo* branches, and repeat the step 3rd and 4th.

## Index

- [Demo01: Linting (JShint)](https://github.com/ruanyf/travis-ci-demo/tree/demo01)
- [Demo02: Testing (Mocha)](https://github.com/ruanyf/travis-ci-demo/tree/demo02)
- [Demo03: Testing (Tape)](https://github.com/ruanyf/travis-ci-demo/tree/demo03)
- [Demo04: After script (Coverall)](https://github.com/ruanyf/travis-ci-demo/tree/demo04)

---

## What is Travis CI?

[Travis CI](https://travis-ci.org/) is a hosted [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) platform that is free for all open source projects hosted on Github.

With a file called `.travis.yml`, you can trigger automated builds with every change to your repo.

## What is `.travis.yml`?

A file called `.travis.yml` in the root of your repository tells Travis CI what to do.

> - What programming language your project uses
> - What commands or scripts you want to be executed before each build (for example, to install or clone your project’s dependencies)
> - What command is used to run your test suite
> - Emails, Campfire and IRC rooms to notify about build failures

You should use this file to customize Travis CI's building behavior. After modifing it, you can use [lint.travis-ci.org](http://lint.travis-ci.org/) to verify this file.

Note that for historical reasons `.travis.yml` needs to be present on all active branches of your project.

## How to write `.travis.yml`?

### 1. Specifying Runtime Versions

The first thing you should do is to specify what languages and runtimes to run your test suite against in the `.travis.yml` file.

```yaml
language: node_js
node_js:
  - "Node"
```

The above `.travis.yml` tells Travis CI that this project should be built with the latest stable version of Node. (You also could use `stable` to replace `node`. They are synonym.)

Travis CI uses nvm to specify Node versions. Any version nvm could recognize can be used in `.travis.yml`.

```yaml
language: node_js
node_js:
  - "4.1"
  - "4.0"
  - "0.12"
  - "0.11"
  - "0.10"
  - "0.8"
  - "0.6"
  - "iojs"
```

This above code will make Travis CI run your tests against the latest version 0.6.x, 0.8.x, 0.10.x, 0.11.x, 0.12.x, 4.0.x, and 4.1.x branch releases, as well as the latest io.js stable release.

Specifying only a major and minor version (e.g., “0.12”) will run using the latest published patch release for that version. If a specific version is not needed, It is encouraged to specify node to run using the latest stable releases.

Official dos has a [list](http://docs.travis-ci.com/user/customizing-the-build/#Specifying-Runtime-Versions) of all languages and runtimes Travis CI supports.

### 2. Default Building Behavior

A build on Travis CI is made up of two steps:

- install: install any dependencies required
- script: run the build script

By default, Travis CI will run

```bash
$ npm install
```

to install your dependencies.

For projects using npm, Travis CI will execute

```bash
$ npm test
```

to run your test suite.

### 3. The Lifecycle

You can run custom commands before the installation step (`before_install`), and before (`before_script`) or after (`after_script`) the script step.

You can perform additional steps when your build succeeds or fails using the `after_success` (such as building documentation, or deploying to a custom server) or `after_failure` (such as uploading log files) options. In both `after_failure` and `after_success`, you can access the build result using the `$TRAVIS_TEST_RESULT` environment variable.

The complete build lifecycle is:

1. `before_install`
1. `install`
1. `before_script`
1. `script`
1. `after_success` or `after_failure`
1. `after_script`
1. OPTIONAL `before_deploy`
1. OPTIONAL `deploy`
1. OPTIONAL `after_deploy`

### 4. Customizing the Installation Step

Travis CI uses the default dependency installation commands depend on the project language to install the dependencies. For Node projects, the default dependency installation commands is `npm install`.

```yaml
install:
  - npm install
```

You can specify your own script to run to install whatever dependencies your project requires in `.travis.yml`.

```yaml
install: ./install-dependencies.sh
```

When one of the steps fails, the build stops immediately and is marked as errored.

You can skip the installation step entirely by adding the following to your `.travis.yml`.

```yaml
install: true
```

### 5. Customizing the Build Step

The default build command depends on the project language. You can overwrite the default build step in .travis.yml:

```yaml
script:
  - bundle exec rake build
  - bundle exec rake builddoc
```

When one of the build commands returns a non-zero exit code, the Travis CI build runs the subsequent commands as well, and accumulates the build result.

If your first step is to run unit tests, followed by integration tests, you may still want to see if the integration tests succeed when the unit tests fail.

You can change this behavior.

```yaml
script: bundle exec rake build && bundle exec rake builddoc
```

This example (note the `&&`) fails immediately when `bundle exec rake build` fails.

If any of the commands in the first four stages of the build lifecycle return a non-zero exit code, the build is broken:

- If `before_install`, `install` or `before_script` return a non-zero exit code, the build is errored and stops immediately.
- If `script` returns a non-zero exit code, the build is failed, but continues to run before being marked as failed.

The `after_success`, `after_failure`, `after_script` and subsequent stages do not affect the the build result.

### 6. Build Timeouts

Because it is very common for test suites or build scripts to hang, Travis CI has specific time limits for each job. If a script or test suite takes longer than 50 minutes (or 120 minutes on travis-ci.com), or if there is not log output for 10 minutes, it is terminated, and a message is written to the build log.

There is no timeout for a build; a build will run as long as all the jobs do as long as each job does not timeout.

### 7. Building Specific Branches

Travis CI uses the `.travis.yml` file from the branch specified by the git commit that triggers the build.

You can tell Travis to build multiple branches using blacklists or whitelists. Specify which branches to build using a whitelist, or blacklist branches that you do not want to be built:

```yaml
# blacklist
branches:
  except:
    - legacy
    - experimental

# whitelist
branches:
  only:
    - master
    - stable
```

If you specify both, only takes precedence over except. By default, gh-pages branch is not built unless you add it to the whitelist.

You can use regular expressions to whitelist or blacklist branches:

```yaml
branches:
  only:
    - master
    - /^deploy-.*$/
```

If you don’t want to run a build for a particular commit, because all you are changing is the README for example, add `[ci skip]` to the git commit message. Commits that have `[ci skip]` anywhere in the commit messages are ignored by Travis CI.

### 8. Deploying your Code

An optional phase in the build lifecycle is deployment.

When deploying files to a provider, prevent Travis CI from resetting your working directory and deleting all changes made during the build ( `git stash --all`) by adding `skip_cleanup` to your `.travis.yml`:

```yaml
deploy:
  skip_cleanup: true
```

You can run steps before a deploy by using the `before_deploy` phase. A non-zero exit code in this command will mark the build as errored.

If there are any steps you’d like to run after the deployment, you can use the `after_deploy` phase.

## Useful Links

- [Building a Node.js project](http://docs.travis-ci.com/user/languages/javascript-with-nodejs/), by Travis CI
- [Customizing the Build](http://docs.travis-ci.com/user/customizing-the-build/), by Travis CI
- [CI-By-Example](https://github.com/buildfirst/ci-by-example), by bevacqua
- [Travis-CI: What, Why, How](http://code.tutsplus.com/tutorials/travis-ci-what-why-how--net-34771), by Sayanee Basu
