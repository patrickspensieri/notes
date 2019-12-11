## Intro to Travis
Be sure to check out the [examples](https://docs.travis-ci.com/user/build-stages/#examples) which include
- Deploying to Heroku
- Combining build stages with build matrices
- Warming up the cache to speed up test times

### Some terminology
- `phase` - the sequential steps of a job. For example, the install phase, comes before the script phase, which comes before the optional deploy phase.
- `job` - an automated process that clones your repository into a virtual environment and then carries out a series of phases such as compiling your code, running tests, etc. A job fails, if the return code of the script phase is non-zero.
- `build` - a group of jobs. For example, a build might have two jobs, each of which tests a project with a different version of a programming language. A build finishes when all of its jobs are finished.
- `stage` - a group of jobs that run in parallel as part of a sequential build process composed of multiple stages.

### What is the job lifecycle?
In short, a job consists of an `install` and `script` parts.
- Travis also [provides different hooks](https://docs.travis-ci.com/user/job-lifecycle/#the-job-lifecycle) such as `before_install`, `after_failure` and `deploy`.

### What is a build stage?
Stages allow you to break up your delivery procecss into a sequence of steps, where one step only starts once its predecessor completes. Stages are composed of jobs, which run in parallel.
- Note that you can [name your build stages](https://docs.travis-ci.com/user/build-stages/#naming-your-build-stages) and the jobs within them.
- You can also [place confitions](https://docs.travis-ci.com/user/build-stages/#naming-your-build-stages) like `branch = master` on when a stage should run.
- Note that jobs run on different VMs, so use external storage to share data.

```yaml
# test stage has two jobs, which run in parallel
# deploy stage has a single job
# you only want to deploy if your tests pass
jobs:
  include:
    - stage: test
      script: ./test 1
    - # stage name not required, will continue to use `test`
      script: ./test 2
    - stage: deploy
      script: ./deploy
```

### What is the build matrix?
A build matrix allows you to run jobs in parallel. There are two ways to specify a build matrix, outlined below.
1. Build a config matrix, where all possible combinations will be run in parallel.

```yaml
# build matrix expands into 8 (2 * 2 * 2) jobs
# here, we use the matrix to run tests in different environments
# this can be done in parallel, as part of a testing stage
rvm:
  - 2.5
  - 2.2
gemfile:
  - gemfiles/Gemfile.rails-3.2.x
  - gemfiles/Gemfile.rails-3.0.x
env:
  - ISOLATED=true
  - ISOLATED=false
```


2. Specify exactly which combinations you'd like to run using `matrix.include`.
- Note that jobs will [inherit the first value for each config](https://docs.travis-ci.com/user/build-matrix/#explicitly-included-jobs-inherit-the-first-value-in-the-array) that isn't explicitly defined.
- You can also [allow some configurations to fail](https://docs.travis-ci.com/user/build-matrix/#rows-that-are-allowed-to-fail).
- You can also [use different programming languages in jobs](https://docs.travis-ci.com/user/build-matrix/#using-different-programming-languages-per-job).

```yaml
matrix:
  include:
  - rvm: 2.5
    gemfile: gemfiles/Gemfile.rails-3.2.x
    env: ISOLATED=false
  - rvm: 2.2
    gemfile: gemfiles/Gemfile.rails-3.0.x
    env: ISOLATED=true
```

---

#### How to speed up builds
- Use a `build matrix` to [run different parts of the suite in parallel](https://docs.travis-ci.com/user/speeding-up-the-build/#parallelizing-your-builds-across-virtual-machines), across various VMS.
- [Configure your test runner](https://docs.travis-ci.com/user/speeding-up-the-build/#parallelizing-your-build-on-one-virtual-machine), such as JUnit to run in parallel, on a single VM.
- [Cache your dependencies](https://docs.travis-ci.com/user/speeding-up-the-build/#caching-the-dependencies), such as `npm`.

#### Travis and monorepos
When you have multiple projects under a single repo, how to configure travis to only run a project's tests if the project changed?

1. Conditionally run test suites depending on git diffs
- see [this comment](https://github.com/travis-ci/travis-ci/issues/3540#issuecomment-330188464)

2. [This repo](https://github.com/sagikazarmark/travis-monorepo-demo/blob/master/.travis.yml) demos having two projects under a single repo, and triggering specific builds.

3. Some [custom scripts](https://travis-ci.community/t/how-to-skip-jobs-based-on-the-files-changed-in-a-subdirectory/2979/10) that compare most recent commits to the most recent commit made to a directory.
