# Use MATLAB with GitHub Actions
With [GitHub&reg; Actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions), you can build and test your MATLAB&reg; project as part of your workflow. For example, you can automatically identify any code issues in your project, run tests and generate test and coverage artifacts, and package your files into a toolbox. The GitHub actions for MATLAB let you run MATLAB code and Simulink&reg; models on [GitHub-hosted](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners) and [self-hosted](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners) runners:
- To use a GitHub-hosted runner, include the [Setup MATLAB](https://github.com/matlab-actions/setup-matlab/) action in your workflow to set up your preferred MATLAB release (R2021a or later) on the runner.
- To use a self-hosted runner, set up a computer with MATLAB on its path and register the runner with GitHub Actions. (On self-hosted UNIX&reg; runners, you can also use the **Setup MATLAB** action instead of having MATLAB already installed.) The runner uses the topmost MATLAB release on the system path to execute your workflow.

## Quick Start
Use [GitHub Actions Workflow Generator for MATLAB](https://matlab-actions.github.io/workflow-generator/) to quickly get started with GitHub Actions for your MATLAB repository. On the workflow generator page, enter your repository. The generator creates a starter workflow that you can review and commit with one click. For more information, see [Generate Starter Workflow for MATLAB](https://github.com/matlab-actions/workflow-generator/blob/main/README.md).

## Overview of Actions
To run MATLAB in your workflow, use these actions when you define your workflow in the `.github/workflows` directory of your repository:

* To set up your GitHub Actions workflow with a specific release of MATLAB, use the [Setup MATLAB](#setup-matlab) action.
* To run a MATLAB build using the MATLAB build tool, use the [Run MATLAB Build](#run-matlab-build) action.
* To run MATLAB and Simulink tests and generate artifacts, use the [Run MATLAB Tests](#run-matlab-tests) action.
* To run MATLAB scripts, functions, and statements, use the [Run MATLAB Command](#run-matlab-command) action.

### Setup MATLAB
Use the **Setup MATLAB** action to set up MATLAB and other MathWorks&reg; products on a GitHub-hosted (Linux&reg;, Windows&reg;, or macOS) runner or self-hosted UNIX (Linux or macOS) runner. The action sets up your preferred MATLAB release (R2021a or later) on the runner. If you do not specify a release, the action sets up the latest release of MATLAB.

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
Each example in this section provides the code that defines a workflow. To run an example, copy the example code to a workflow file and store that file in the `.github/workflows` directory of your repository. A workflow file can have any name, but it must have either a `.yml` or `.yaml` file extension (for example, `matlab.yml`).

### Run Default Tasks in Build File
On a self-hosted runner that has MATLAB installed, run the default tasks in a build file named `buildfile.m` in the root of your repository as well as all the tasks on which they depend. To run the tasks, specify the **Run MATLAB Build** action in your workflow. (The **Run MATLAB Build** action is supported in MATLAB R2022b and later.)

```yaml
name: Run Default Tasks in Build File
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

### Generate Test and Coverage Artifacts
Using the latest release of MATLAB on a GitHub-hosted runner, run the tests in your [MATLAB project](https://www.mathworks.com/help/matlab/projects.html) and generate test results in JUnit-style XML format and code coverage results in Cobertura XML format. To set up the latest release of MATLAB on the runner, specify the **Setup MATLAB** action in your workflow. To run the tests and generate the artifacts, specify the **Run MATLAB Tests** action.

```yaml
name: Generate Test and Coverage Artifacts
on: [push]
jobs:
  my-job:
    name: Run MATLAB Tests
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      - name: Set up MATLAB
        uses: matlab-actions/setup-matlab@v2
      - name: Run tests
        uses: matlab-actions/run-tests@v2
        with:
          test-results-junit: test-results/results.xml
          code-coverage-cobertura: code-coverage/coverage.xml
```

### Run MATLAB Script
Run the commands in a file named `myscript.m` in the root of your repository using MATLAB R2024a on a GitHub-hosted runner. To set up the specified release of MATLAB on the runner, specify the **Setup MATLAB** action with its `release` input in your workflow. To run the script, specify the **Run MATLAB Command** action.

```yaml
name: Run MATLAB Script
on: [push]
jobs:
  my-job:
    name: Run MATLAB Script
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      - name: Set up MATLAB
        uses: matlab-actions/setup-matlab@v2
        with:
          release: R2024a
      - name: Run script
        uses: matlab-actions/run-command@v2
        with:
          command: myscript
```

### Specify MATLAB Release on Self-Hosted Runner
When you use the **Run MATLAB Build**, **Run MATLAB Tests**, or **Run MATLAB Command** action in your workflow, the runner uses the topmost MATLAB release on the system path. The action fails if the runner cannot find any release of MATLAB on the path.

You can prepend your preferred release of MATLAB to the `PATH` system environment variable of the self-hosted runner. For example, prepend MATLAB R2020b to the path and use it to run a script. The step depends on your operating system and MATLAB root folder.

```YAML
name: Specify MATLAB Release on Self-Hosted Runner
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
        run: echo "C:\Program Files\MATLAB\R2020b\bin" | Out-File -FilePath $env:GITHUB_PATH -Encoding utf8 -Append     
      - name: Prepend MATLAB to PATH on Linux
        if: runner.os == 'Linux'
        run: echo "/usr/local/MATLAB/R2020b/bin" >> $GITHUB_PATH
      - name: Prepend MATLAB to PATH on macOS
        if: runner.os == 'macOS'
        run: echo "/Applications/MATLAB_R2020b.app/bin" >> $GITHUB_PATH
      - name: Run script
        uses: matlab-actions/run-command@v2
        with:
          command: myscript
```

### Use MATLAB Batch Licensing Token
When you define a workflow using the [Setup MATLAB](https://github.com/matlab-actions/setup-matlab/) action, you need a [MATLAB batch licensing token](https://github.com/mathworks-ref-arch/matlab-dockerfile/blob/main/alternates/non-interactive/MATLAB-BATCH.md#matlab-batch-licensing-token) if your project is private or if your workflow uses transformation products, such as MATLAB Coder&trade; and MATLAB Compiler&trade;. Batch licensing tokens are strings that enable MATLAB to start in noninteractive environments. You can request a token by submitting the [MATLAB Batch Licensing Pilot](https://www.mathworks.com/support/batch-tokens.html) form. 

To use a MATLAB batch licensing token:

1. Set the token as a secret. For more information about secrets, see [Using secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions).
2. Map the secret to an environment variable named `MLM_LICENSE_TOKEN` in your workflow. 

For example, use the latest release of MATLAB on a GitHub-hosted runner to run the tests in your private project. To set up the latest release of MATLAB on the runner, specify the **Setup MATLAB** action in your workflow. To run the tests, specify the **Run MATLAB Tests** action. In this example, `MyToken` is the name of the secret that holds the batch licensing token.

```YAML
name: Use MATLAB Batch Licensing Token
on: [push]
env:
  MLM_LICENSE_TOKEN: ${{ secrets.MyToken }}
jobs:
  my-job:
    name: Run MATLAB Tests in Private Project
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      - name: Set up MATLAB
        uses: matlab-actions/setup-matlab@v2
      - name: Run tests
        uses: matlab-actions/run-tests@v2
```

### Use Virtual Display on Linux Runner
GitHub-hosted Linux runners do not provide a display. To run MATLAB code that requires a display, such as tests that interact with an app UI, first set up a virtual display on the runner by starting an Xvfb display server. For example, set up a virtual server to run the tests for an app created in MATLAB.

```YAML
name: Use Virtual Display on Linux Runner
on: [push]
jobs:
  my-job:
    name: Test an App
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      - name: Start virtual display server
        if: runner.os == 'Linux'
        run: |
          sudo apt-get install -y xvfb
          Xvfb :99 &
          echo "DISPLAY=:99" >> $GITHUB_ENV
      - name: Set up MATLAB
        uses: matlab-actions/setup-matlab@v2
      - name: Run tests
        uses: matlab-actions/run-tests@v2
```

## Notes
* To use the GitHub actions for MATLAB, enable GitHub Actions for your repository. For more information about GitHub Actions permissions, see [Managing GitHub Actions settings for a repository](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository).
* By default, when you use the **Run MATLAB Build**, **Run MATLAB Tests**, or **Run MATLAB Command** action, the root of your repository serves as the MATLAB startup folder. To run your MATLAB code using a different folder, specify the `-sd` startup option or include the `cd` command when using the **Run MATLAB Command** action.
* The **Run MATLAB Build** action uses the `-batch` option to invoke the [`buildtool`](https://www.mathworks.com/help/matlab/ref/buildtool.html) command. In addition, in MATLAB R2019a and later, the **Run MATLAB Tests** and **Run MATLAB Command** actions use  the `-batch` option to start MATLAB noninteractively. MATLAB settings do not persist across different MATLAB sessions launched with the `-batch` option. To run code that requires the same settings, use a single action.
* When you use the **Setup MATLAB**, **Run MATLAB Build**, **Run MATLAB Tests**, and **Run MATLAB Command** actions, you execute third-party code that is licensed under separate terms.

## See Also
- [Continuous Integration with MATLAB and Simulink](https://www.mathworks.com/solutions/continuous-integration.html)
- [Continuous Integration with MATLAB on CI Platforms](https://www.mathworks.com/help/matlab/matlab_prog/continuous-integration-with-matlab-on-ci-platforms.html)

## Contact Us
If you have any questions or suggestions, contact MathWorks at [continuous-integration@mathworks.com](mailto:continuous-integration@mathworks.com).
