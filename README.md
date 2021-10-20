# Use MATLAB with GitHub Actions
With [GitHub&reg; actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions) for MATLAB&reg;, you can run MATLAB scripts, functions, and statements as part of your workflow. You also can run your MATLAB and Simulink&reg; tests and generate artifacts such as JUnit test results and Cobertura code coverage reports. The actions let you run MATLAB code and Simulink models on [self-hosted](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners) or [GitHub-hosted](https://docs.github.com/en/free-pro-team@latest/actions/reference/specifications-for-github-hosted-runners) runners:

- To use a self-hosted runner, you must set up a computer with MATLAB (R2013b or later) as your runner. The runner uses the topmost MATLAB version on the system path to execute your workflow.

- To use a GitHub-hosted runner, you must include the [Setup MATLAB](#setup-matlab) action in your workflow to set up MATLAB on the runner. Currently, this action is available only for public projects. It does not set up transformation products, such as MATLAB Coder&trade; and MATLAB Compiler&trade;.

## Overview of Actions
To run MATLAB in your workflow, use the appropriate actions when you define your workflow in the `.github/workflows` directory of your repository:

* To set up MATLAB on a GitHub-hosted runner, use the [Setup MATLAB](#setup-matlab) action.
* To execute a MATLAB script, function, or statement, use the [Run MATLAB Command](#run-matlab-command) action.
* To run MATLAB and Simulink tests and generate artifacts, use the [Run MATLAB Tests](#run-matlab-tests) action.

### Setup MATLAB
Use the **Setup MATLAB** action when you want to run MATLAB code and Simulink models on a GitHub-hosted runner. The action sets up your specified MATLAB release (R2020a or later) on a Linux&reg; virtual machine. If you do not specify a release, the action sets up the latest release of MATLAB.

When you define your workflow, specify this action as `matlab-actions/setup-matlab@v1`. For more information, see [Action for Setting Up MATLAB on GitHub-Hosted Runner](https://github.com/matlab-actions/setup-matlab/).

### Run MATLAB Command
Use the **Run MATLAB Command** action to run MATLAB scripts, functions, and statements. You can use this action to flexibly customize your test run or add a MATLAB related step to your workflow. 

When you define your workflow, specify this action as `matlab-actions/run-command@v1`. For more information, see [Action for Running MATLAB Commands](https://github.com/matlab-actions/run-command/).

### Run MATLAB Tests
Use the **Run MATLAB Tests** action to automatically run tests authored using the MATLAB Unit Testing Framework or Simulink Test&trade;. You can use this action with optional inputs to generate various test and coverage artifacts.

When you define your workflow, specify this action as `matlab-actions/run-tests@v1`. For more information, see [Action for Running MATLAB Tests](https://github.com/matlab-actions/run-tests/).

## Examples

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
        uses: actions/checkout@v2
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
        uses: actions/checkout@v2
      - name: Run script
        uses: matlab-actions/run-command@v1
        with:
          command: myscript
```

## Notes
* To use the GitHub actions for MATLAB, enable GitHub Actions for your repository. For more information about GitHub Actions permissions, see [Disabling or limiting GitHub Actions for a repository](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/disabling-or-limiting-github-actions-for-a-repository).
* When you use the **Setup MATLAB**, **Run MATLAB Command**, and **Run MATLAB Tests** actions, you execute third-party code that is licensed under separate terms.

## See Also
- [Continuous Integration with MATLAB and Simulink](https://www.mathworks.com/solutions/continuous-integration.html)
- [Continuous Integration with MATLAB on CI Platforms](https://www.mathworks.com/help/matlab/matlab_prog/continuous-integration-with-matlab-on-ci-platforms.html)

## Contact Us
If you have any questions or suggestions, please contact MathWorks&reg; at [continuous-integration@mathworks.com](mailto:continuous-integration@mathworks.com).
