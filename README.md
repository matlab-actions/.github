# GitHub Actions for MATLAB
With [GitHub&reg; actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions) for MATLAB&reg;, you can run MATLAB scripts, functions, and statements as part of your workflow. You also can run your MATLAB and Simulink&reg; tests and generate artifacts such as JUnit test results and Cobertura code coverage reports. The actions let you run MATLAB code and Simulink models on [self-hosted](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners) or [GitHub-hosted](https://docs.github.com/en/free-pro-team@latest/actions/reference/specifications-for-github-hosted-runners) runners:

- If you want to use a self-hosted runner, you must set up a computer with MATLAB (R2013b or later) as your runner. The runner uses the first MATLAB version on the system path to execute your workflow.

- If you want to use a GitHub-hosted runner, you must include the [Set Up MATLAB](#set-up-matlab) action in your workflow to install MATLAB on the runner. Currently, this action is available only for public projects and does not include transformation products, such as MATLAB Coder&trade; and MATLAB Compiler&trade;.

## Overview of Actions
To run MATLAB in your workflow, use the appropriate actions when you define your workflow in the `.github/workflows` directory of your repository:

* To install MATLAB on a Linux&reg;-based GitHub-hosted runner, use the [Set Up MATLAB](#set-up-matlab) action.
* To execute a MATLAB script, function, or statement on a self-hosted or GitHub-hosted runner, use the [Run MATLAB Command](#run-matlab-command) action.
* To run MATLAB and Simulink tests and generate artifacts on a self-hosted or GitHub-hosted runner, use the [Run MATLAB Tests](#run-matlab-tests) action.

### Set Up MATLAB
Use the **Set Up MATLAB** action when you want to run MATLAB code and Simulink models on a GitHub-hosted runner. The action installs your specified MATLAB release (R2020a or later) on a Linux virtual machine. If you do not specify a release, the action installs the latest release of MATLAB.

When you define your workflow, you can specify this action as `matlab-actions/setup-matlab@v0`. For more information, see [Action for Installing MATLAB on GitHub-Hosted Runner](https://github.com/matlab-actions/setup-matlab/).

### Run MATLAB Command
Use the **Run MATLAB Command** action to run MATLAB scripts, functions, and statements tailored to your specific needs. You can use this action to flexibly customize your test run or add a MATLAB-related step to your workflow. 

When you define your workflow, you can specify this action as `matlab-actions/run-command@v0`. For more information, see [Action for Running MATLAB Commands](https://github.com/matlab-actions/run-command/).

### Run MATLAB Tests
Use the **Run MATLAB Tests** action to automatically run tests authored using the MATLAB Unit Testing Framework or Simulink Test&trade;. You can use this action with optional inputs to generate different types of test artifacts.

When you define your workflow, you can specify this action as `matlab-actions/run-tests@v0`. For more information, see [Action for Running MATLAB Tests](https://github.com/matlab-actions/run-tests/).

## Examples

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
        uses: actions/checkout@v2
      - name: Run script
        uses: matlab-actions/run-command@v0
        with:
          command: myscript
```

### Run MATLAB Tests on GitHub-Hosted Runner
Set up a GitHub-hosted runner to automatically run the tests in your [MATLAB project](https://www.mathworks.com/help/matlab/projects.html) and generate a JUnit test results report and a Cobertura code coverage report. To install the latest release of MATLAB on the runner, specify the **Set Up MATLAB** action in your workflow. To run the tests and generate the artifacts, specify the **Run MATLAB Tests** action.

```yaml
name: Run MATLAB Tests on GitHub-Hosted Runner
on: [push]
jobs:
  my-job:
    name: Run MATLAB Tests and Generate Artifacts
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v2
      - name: Install MATLAB
        uses: matlab-actions/setup-matlab@v0
      - name: Run tests and generate artifacts
        uses: matlab-actions/run-tests@v0
        with:
          test-results-junit: test-results/results.xml
          code-coverage-cobertura: code-coverage/coverage.xml
```

## Notes
* To use the GitHub actions for MATLAB, GitHub Actions must be enabled for your repository. For more information about GitHub Actions permissions, see [Disabling or limiting GitHub Actions for a repository](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/disabling-or-limiting-github-actions-for-a-repository).
* By running the **Set Up MATLAB**, **Run MATLAB Command**, and **Run MATLAB Tests** actions, you will be executing third-party code that is licensed under separate terms.

## See Also
- [Continuous Integration with MATLAB and Simulink](https://www.mathworks.com/solutions/continuous-integration.html)
- [Continuous Integration with MATLAB on CI Platforms](https://www.mathworks.com/help/matlab/matlab_prog/continuous-integration-with-matlab-on-ci-platforms.html)

## Contact Us
If you have any questions or suggestions, please contact MathWorks&reg; at [continuous-integration@mathworks.com](mailto:continuous-integration@mathworks.com).
