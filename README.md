
# Flutter SonarCloud Integration Demo

This repository contains a simple Flutter application that demonstrates how to integrate [SonarCloud](https://sonarcloud.io/) for automated code quality and security analysis. The project walks through the setup process for SonarCloud, utilizing **GitHub Actions** to automate the analysis workflow. This is part of an article illustrating how to enhance your Flutter codebase using SonarCloud.

## Features

- Basic Flutter app with core widgets and a home screen.
- Integrated SonarCloud analysis for Dart, checking code quality, detecting bugs, code smells, and security vulnerabilities.
- Automated code analysis using GitHub Actions and a custom bash script to handle Flutter dependencies.

## Project Structure

```
/lib
  └── main.dart            # Main entry point of the Flutter application
/test
  └── widget_test.dart     # Simple widget test example
.github/workflows
  └── sonarcloud.yml       # GitHub Actions workflow for SonarCloud analysis
sonar-project.properties   # SonarCloud configuration file
```

## Prerequisites

- [Flutter SDK](https://flutter.dev/docs/get-started/install) installed on your local machine.
- A [SonarCloud](https://sonarcloud.io/) account. If you're using SonarCloud for the first time, you can take advantage of the **free plan** for open-source projects.
- A **GitHub repository** where this Flutter project is hosted.

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/flutter-sonarcloud-demo.git
cd flutter-sonarcloud-demo
```

### 2. Set Up SonarCloud for the Project

1. Log in to your [SonarCloud account](https://sonarcloud.io/).
2. Create a new project in SonarCloud linked to this GitHub repository.
3. Configure the `sonar-project.properties` file with your project details:

```properties
sonar.projectKey=your_project_key
sonar.organization=your_organization
sonar.exclusions=**/test/**
sonar.dart.lcov.reportPaths=coverage/lcov.info
```

### 3. GitHub Actions Workflow

The `sonarcloud.yml` workflow file located in `.github/workflows/` is set up to run SonarCloud analysis on every push and pull request to the `main` branch.

If you're using a **paid plan** or **14-day trial**, make sure your SonarCloud token is securely stored as a **GitHub secret**:

1. Go to your GitHub repository > **Settings** > **Secrets and variables** > **Actions** > **New repository secret**.
2. Add your **SONAR_TOKEN** as the key and the generated token from SonarCloud as the value.

### 4. Run the App Locally

Ensure that Flutter is properly installed and set up on your machine:

```bash
flutter pub get
flutter run
```

### 5. Run Tests

This project includes a basic widget test to verify your setup. You can run the tests with:

```bash
flutter test
```

OR you can generate a coverage report using the following command:

```bash     
flutter test --coverage
```

## GitHub Actions: Custom Bash Script for Dart Analysis

Due to some limitations with SonarCloud's GitHub Action running in Docker and not being able to access Flutter dependencies, a **bash script** is used for Dart analysis in this workflow. This script is included in the `.github/workflows/sonarcloud.yml` file.

```yaml
- name: Analyze Flutter project with SonarCloud
  run: |
    export SONAR_SCANNER_VERSION=6.2.1.4610
    export SONAR_SCANNER_HOME=$HOME/.sonar/sonar-scanner-$SONAR_SCANNER_VERSION-linux-x64
    curl --create-dirs -sSLo $HOME/.sonar/sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$SONAR_SCANNER_VERSION-linux-x64.zip
    unzip -o $HOME/.sonar/sonar-scanner.zip -d $HOME/.sonar/
    export PATH=$SONAR_SCANNER_HOME/bin:$PATH
    export SONAR_SCANNER_OPTS="-server"
    sonar-scanner \
      -Dsonar.organization=your_organization \
      -Dsonar.projectKey=your_project_key \
      -Dsonar.sources=. \
      -Dsonar.host.url=https://sonarcloud.io
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
