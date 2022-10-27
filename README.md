# spring-gumball ci/cd example

### This example demonstrates the following two GitHub Action Workflows.

* [Building and testing Java with Gradle](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-gradle) 
* [Deploying to Google Kubernetes Engine](https://docs.github.com/en/actions/deployment/deploying-to-your-cloud-provider/deploying-to-google-kubernetes-engine)

### Build Dependencies

* Gradle 5.6
* JDK 11



## CI Workflow (Part 1)

* https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-gradle 

![java-gradle](./images/01-building-and-testing-java-with-gradle.png)


### Building and testing Java with Gradle

You can create a continuous integration (CI) workflow in GitHub Actions to build and test your Java project with Gradle.

#### Introduction

This guide shows you how to create a workflow that performs continuous integration (CI) for your Java project using the Gradle build system. The workflow you create will allow you to see when commits to a pull request cause build or test failures against your default branch; this approach can help ensure that your code is always healthy. You can extend your CI workflow to cache files and upload artifacts from a workflow run.

GitHub-hosted runners have a tools cache with pre-installed software, which includes Java Development Kits (JDKs) and Gradle. For a list of software and the pre-installed versions for JDK and Gradle, see [Specifications for GitHub-hosted runners](https://docs.github.com/en/actions/reference/specifications-for-github-hosted-runners/#supported-software).

#### Prerequisites

You should be familiar with YAML and the syntax for GitHub Actions. For more information, see:

* [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
* [Learn GitHub Actions](https://docs.github.com/en/actions/learn-github-actions)

We recommend that you have a basic understanding of Java and the Gradle framework. For more information, see [Getting Started](https://docs.gradle.org/current/userguide/getting_started.html) in the Gradle documentation.

#### Using the Gradle starter workflow

GitHub provides a Gradle starter workflow that will work for most Gradle-based Java projects. For more information, see the [Gradle starter workflow](https://github.com/actions/starter-workflows/blob/main/ci/gradle.yml).

To get started quickly, you can choose the preconfigured Gradle starter workflow when you create a new workflow. For more information, see the [GitHub Actions quickstart](https://docs.github.com/en/actions/quickstart).

You can also add this workflow manually by creating a new file in the **.github/workflows** directory of your repository.

**Set up your workflow to trigger on push and pr on main branch and optionally upload build artifact (i.e. jar file).**

For Example: **.github/workflows/gradle.yml**

```
# This workflow will build a Java project with Gradle
# See Also: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-gradle

name: Java CI with Gradle

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    # The checkout step downloads a copy of your repository on the runner.
    - uses: actions/checkout@v2

    # The setup-java step configures the Java 11 JDK by Adoptium.
    - name: Set up JDK 11
      uses: actions/setup-java@v2
      with:
        java-version: '11'
        distribution: 'adopt'

    # Make sure gradle wrapper is executable
    - name: Grant execute permission for gradlew
      run: chmod +x gradlew

    # The "Build with Gradle" step does a build using the gradle/gradle-build-action 
    # action provided by the Gradle organization on GitHub. The action takes care of 
    # invoking Gradle, collecting results, and caching state between jobs. 
    # For more information, See: https://github.com/gradle/gradle-build-action.      
    - name: Build with Gradle
      run: ./gradlew build

    # List the Build output folder to confirm JAR file was built
    - name: Build Result
      run: ls build/libs

    # Gradle will usually create output files like JARs, EARs, or WARs in the build/libs directory. 
    # You can upload the contents of that directory using the upload-artifact action.
    # See: https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts
    - name: Upload a Build Artifact
      uses: actions/upload-artifact@v2.2.3
      with:
        name: spring-gumball
        path: build/libs/spring-gumball-2.0.jar
```


Make a change to the code and commit to main branch to trigger the action. Take screenhots of your result.  For Example:

![](./images/02-ci-workflow-gradle-build-manifest.png)
![](./images/03-ci-workflow-commit-triggers-build.png)
![](./images/04-ci-workflow-github-action-build-results-part-1.png)
![](./images/05-ci-workflow-github-action-build-results-part-2.png)






