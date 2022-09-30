# Use MATLAB with GitHub Actions
With [GitHub&reg; Actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions), you can build and test your MATLAB&reg; project as part of your workflow. For example, you can automatically identify any code issues in your project, run tests and generate test and coverage artifacts, or package your files into a toolbox. The actions let you run MATLAB code and Simulink&reg; models on [self-hosted](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners) or [GitHub-hosted](https://docs.github.com/en/free-pro-team@latest/actions/reference/specifications-for-github-hosted-runners) runners:

- To use a self-hosted runner, you must set up a computer with MATLAB as your runner. The runner uses the topmost MATLAB version on the system path to execute your workflow.

- To use a GitHub-hosted runner, you must include the [Setup MATLAB](#setup-matlab) action in your workflow to set up MATLAB on the runner. Currently, this action is available only for public projects. It does not set up transformation products, such as MATLAB Coder&trade; and MATLAB Compiler&trade;.

## Overview of Actions
To run MATLAB in your workflow, use the appropriate actions when you define your workflow in the `.github/workflows` directory of your repository:

* To set up MATLAB on a GitHub-hosted runner, use the [Setup MATLAB](#setup-matlab) action.
* To run a MATLAB build, use the [Run MATLAB Build](#run-matlab-build) action.
* To run MATLAB and Simulink tests and generate artifacts, use the [Run MATLAB Tests](#run-matlab-tests) action.
* To run a MATLAB script, function, or statement, use the [Run MATLAB Command](#run-matlab-command) action.

### Setup MATLAB
Use the **Setup MATLAB** action when you want to run MATLAB code and Simulink models on a GitHub-hosted runner. The action sets up your specified MATLAB release (R2020a or later) on a Linux&reg; virtual machine. If you do not specify a release, the action sets up the latest release of MATLAB.

When you define your workflow, specify this action as `matlab-actions/setup-matlab@v1`. For more information, see [Action for Setting Up MATLAB on GitHub-Hosted Runner](https://github.com/matlab-actions/setup-matlab/).

### Run MATLAB Build
Use the **Run MATLAB Build** action to invoke the MATLAB build tool and run software-build tasks, such as identifying code issues, running tests, or packaging a toolbox. To use this action, you need MATLAB R2022b or a later release.

When you define your workflow, specify this action as `matlab-actions/run-build@v1`. For more information, see [Action for Running MATLAB Builds](https://github.com/matlab-actions/run-build).

### Run MATLAB Tests
Use the **Run MATLAB Tests** action to automatically run tests authored using the MATLAB unit testing framework or Simulink Test&trade;. You can use this action with optional inputs to generate various test and coverage artifacts.

When you define your workflow, specify this action as `matlab-actions/run-tests@v1`. For more information, see [Action for Running MATLAB Tests](https://github.com/matlab-actions/run-tests/).

### Run MATLAB Command
Use the **Run MATLAB Command** action to run MATLAB scripts, functions, and statements. You can use this action to flexibly customize your test run or add a step in MATLAB to your workflow. 

When you define your workflow, specify this action as `matlab-actions/run-command@v1`. For more information, see [Action for Running MATLAB Commands](https://github.com/matlab-actions/run-command/).

## Examples

### Run MATLAB Build on Self-Hosted Runner
Starting in R2022b, the **Run MATLAB Build** action lets you run builds using the MATLAB build tool. You can use this action to run the tasks specified in a file named  `buildfile.m` in the root of your repository. For example, use a self-hosted runner to run the default tasks in your build plan as well as all the tasks on which they depend. To run the tasks, specify the **Run MATLAB Build** action in your workflow.

```yaml
name: Run MATLAB Build on Self-Hosted Runner
on: [push]
jobs:
  my-job:
    name: Run MATLAB Build
    runs-on: self-hosted
    steps:
      - name: Check out repository
        uses: actions/checkout@v3
      - name: Run build
        uses: matlab-actions/run-build@v1
```

### Run MATLAB Tests on GitHub-Hosted Runner
Set up a GitHub-hosted runner to automatically run the tests in your [MATLAB project](https://www.mathworks.com/help/matlab/projects.html) and generate a JUnit test results report and a Cobertura code coverage report. To set up the latest release of MATLAB on the runner, specify the **Setup MATLAB** action in your workflow. To run the tests and generate the artifacts, specify the **Run MATLAB Tests** action.

```yaml
name: Run MATLAB Tests on GitHub-Hosted Runner
on: [push]
jobs:
  my-job:
    name: Run MATLAB Tests and Generate Artifacts
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v3
      - name: Set up MATLAB
        uses: matlab-actions/setup-matlab@v1
      - name: Run tests and generate artifacts
        uses: matlab-actions/run-tests@v1
        with:
          test-results-junit: test-results/results.xml
          code-coverage-cobertura: code-coverage/coverage.xml
```

### Run MATLAB Script on Self-Hosted Runner
Use a self-hosted runner to run the commands in a file named `myscript.m` in the root of your repository. To run the commands, specify the **Run MATLAB Command** action in your workflow.

```yaml
name: Run MATLAB Script on Self-Hosted Runner
on: [push]
jobs:
  my-job:
    name: Run MATLAB Script
    runs-on: self-hosted
    steps:
      - name: Check out repository
        uses: actions/checkout@v3
      - name: Run script
        uses: matlab-actions/run-command@v1
        with:
          command: myscript
```

## Notes
* To use the GitHub actions for MATLAB, enable GitHub Actions for your repository. For more information about GitHub Actions permissions, see [Managing GitHub Actions settings for a repository](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository).
* The **Run MATLAB Build** action uses the `-batch` option to invoke the [`buildtool`](https://www.mathworks.com/help/matlab/ref/buildtool.html) command. In addition, in MATLAB R2019a and later, the **Run MATLAB Tests** and **Run MATLAB Command** actions use  the `-batch` option to start MATLAB noninteractively. Preferences do not persist across different MATLAB sessions launched with the `-batch` option. To run code that requires the same preferences, use a single action.
* When you use the **Setup MATLAB**, **Run MATLAB Build**, **Run MATLAB Tests**, and **Run MATLAB Command** actions, you execute third-party code that is licensed under separate terms.

## See Also
- [Continuous Integration with MATLAB and Simulink](https://www.mathworks.com/solutions/continuous-integration.html)
- [Continuous Integration with MATLAB on CI Platforms](https://www.mathworks.com/help/matlab/matlab_prog/continuous-integration-with-matlab-on-ci-platforms.html)

## Contact Us
If you have any questions or suggestions, please contact MathWorks&reg; at [continuous-integration@mathworks.com](mailto:continuous-integration@mathworks.com).
