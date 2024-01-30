# Use MATLAB with GitHub Actions
With [GitHub&reg; Actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions), you can build and test your MATLAB&reg; project as part of your workflow. For example, you can automatically identify any code issues in your project, run tests and generate test and coverage artifacts, and package your files into a toolbox. The GitHub actions for MATLAB let you run MATLAB code and Simulink&reg; models on [self-hosted](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners) or [GitHub-hosted](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners) runners. The actions use the topmost MATLAB version on the system path.

## Overview of Actions
To run MATLAB in your workflow, use the appropriate actions when you define your workflow in the `.github/workflows` directory of your repository:

* To set up your GitHub Actions workflow with a specific version of MATLAB, use the [Setup MATLAB](#setup-matlab) action.
* To run a MATLAB build, use the [Run MATLAB Build](#run-matlab-build) action.
* To run MATLAB and Simulink tests and generate artifacts, use the [Run MATLAB Tests](#run-matlab-tests) action.
* To run MATLAB scripts, functions, and statements, use the [Run MATLAB Command](#run-matlab-command) action.

### Setup MATLAB
Use the **Setup MATLAB** action to run MATLAB code and Simulink models with a specific version of MATLAB. The action sets up your specified MATLAB release (R2020b or later) on a Linux&reg;, Windows&reg;, or macOS&reg; virtual machine. If you do not specify a release, the action sets up the latest release of MATLAB.

When you define your workflow, specify this action as `matlab-actions/setup-matlab@v2`. For more information, see [Action for Setting Up MATLAB](https://github.com/matlab-actions/setup-matlab/).

### Run MATLAB Build
Use the **Run MATLAB Build** action to invoke the MATLAB build tool and run build tasks, such as identifying code issues, running tests, and packaging a toolbox. To use this action, you need MATLAB R2022b or a later release.

When you define your workflow, specify this action as `matlab-actions/run-build@v2`. For more information, see [Action for Running MATLAB Builds](https://github.com/matlab-actions/run-build).

### Run MATLAB Tests
Use the **Run MATLAB Tests** action to automatically run tests authored using the MATLAB unit testing framework or Simulink Test&trade;. You can use this action with optional inputs to generate various test and coverage artifacts.

When you define your workflow, specify this action as `matlab-actions/run-tests@v2`. For more information, see [Action for Running MATLAB Tests](https://github.com/matlab-actions/run-tests/).

### Run MATLAB Command
Use the **Run MATLAB Command** action to run MATLAB scripts, functions, and statements. You can use this action to flexibly customize your test run or add a step in MATLAB to your workflow. 

When you define your workflow, specify this action as `matlab-actions/run-command@v2`. For more information, see [Action for Running MATLAB Commands](https://github.com/matlab-actions/run-command/).

## Examples

### Run MATLAB Build on Self-Hosted Runner
Starting in R2022b, the **Run MATLAB Build** action lets you run a build using the MATLAB build tool. You can use this action to run the tasks specified in a file named  `buildfile.m` in the root of your repository. For example, use a self-hosted runner to run the default tasks in your build plan as well as all the tasks on which they depend. To run the tasks, specify the **Run MATLAB Build** action in your workflow.

```yaml
name: Run MATLAB Build on Self-Hosted Runner
on: [push]
jobs:
  my-job:
    name: Run MATLAB Build
    runs-on: self-hosted
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      - name: Run build
        uses: matlab-actions/run-build@v2
```

### Run MATLAB Tests on GitHub-Hosted Runner
Set up a GitHub-hosted runner to automatically run the tests in your [MATLAB project](https://www.mathworks.com/help/matlab/projects.html) and generate test results in JUnit-style XML format and code coverage results in Cobertura XML format. To set up the latest release of MATLAB on the runner, specify the **Setup MATLAB** action in your workflow. To run the tests and generate the artifacts, specify the **Run MATLAB Tests** action.

```yaml
name: Run MATLAB Tests on GitHub-Hosted Runner
on: [push]
jobs:
  my-job:
    name: Run MATLAB Tests and Generate Artifacts
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      - name: Set up MATLAB
        uses: matlab-actions/setup-matlab@v2
      - name: Run tests and generate artifacts
        uses: matlab-actions/run-tests@v2
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
        uses: actions/checkout@v4
      - name: Run script
        uses: matlab-actions/run-command@v2
        with:
          command: myscript
```

### Specify MATLAB Version on Self-Hosted Runner
When you use the **Run MATLAB Build**, **Run MATLAB Tests**, or **Run MATLAB Command** action in your workflow, the runner uses the topmost MATLAB version on the system path. The build fails if the runner cannot find any version of MATLAB on the path.

You can prepend your preferred version of MATLAB to the `PATH` environment variable of the self-hosted runner. For example, prepend MATLAB R2023b to the path and use it to run your script. The step depends on your operating system and MATLAB root folder.

```YAML
name: Run MATLAB Script on Self-Hosted Runner
on: [push]
jobs:
  my-job:
    name: Run MATLAB Script
    runs-on: self-hosted
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      - name: Prepend MATLAB to PATH on Windows (PowerShell)
        if: runner.os == 'Windows'
        run: echo "C:\Program Files\MATLAB\R2023b\bin" | Out-File -FilePath $env:GITHUB_PATH -Encoding utf8 -Append     
      - name: Prepend MATLAB to PATH on Linux
        if: runner.os == 'Linux'
        run: echo "/usr/local/MATLAB/R2023b/bin" >> $GITHUB_PATH
      - name: Prepend MATLAB to PATH on macOS
        if: runner.os == 'macOS'
        run: echo "/Applications/MATLAB_R2023b.app/bin" >> $GITHUB_PATH
      - name: Run script
        uses: matlab-actions/run-command@v2
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
