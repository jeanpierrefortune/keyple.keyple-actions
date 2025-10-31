# Eclipse Keyple Reusable Workflows and Composite Actions

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains all **GitHub Actions** and **reusable workflows** for the `eclipse-keyple` organization. It's structured to separate **custom composite actions**, **reusable workflows**, and to clearly mark **deprecated** items.

---

## 🔁 Reusable Workflows

These workflows can be invoked in any org repository via `workflow_call`.

| Name                           | Description                                                                  | Path                                                           |
|--------------------------------|------------------------------------------------------------------------------|----------------------------------------------------------------|
| Build and Test                 | Build and test Java/Gradle projects with license verification               | `.github/workflows/reusable-build-and-test.yml`               |
| Publish Snapshot               | Publish snapshot versions to Maven Central                                   | `.github/workflows/reusable-publish-snapshot.yml`             |
| Publish Release                | Publish official releases to Maven Central with GPG signing                  | `.github/workflows/reusable-publish-release.yml`              |
| Publish Doxygen                | Generate and publish Doxygen documentation (C++) to `doc` branch             | `.github/workflows/reusable-publish-doxygen.yml`              |

**Workflow Logic**:
- **Build and Test**: Sets up Java environment, runs Gradle build, and verifies dependency license compliance.
- **Publish Snapshot**: Validates version is not already released, builds the project, publishes to Maven Central Snapshots, and updates documentation.
- **Publish Release**: Verifies version consistency, signs artifacts with GPG, publishes to Maven Central, triggers automatic upload, and updates documentation.
- **Publish Doxygen**: Extracts repository metadata and invokes the `doxygen` action to generate and deploy documentation.

---

## 🔧 Internal Composite Actions

These actions live under `actions/` and encapsulate reusable complex workflows.

| Name                  | Description                                                                | Path                                        | Status        |
|-----------------------|----------------------------------------------------------------------------|---------------------------------------------|---------------|
| doxygen               | Generate and publish Doxygen documentation (C++ API)                       | `actions/doxygen/action.yml`                | **Active**    |
| dash-licenses         | Verify dependency license compliance using Eclipse Dash                    | `actions/dash-licenses/action.yml`          | **Active**    |
| update-documentation  | Generate and push Javadoc to `doc` branch                                  | `actions/update-documentation/action.yml`   | **Active**    |

**Composite Action Logic**:
- **doxygen**: Installs Python and dependencies, validates and patches Doxyfile, generates documentation via Doxygen, prepares versioned directory structure, deploys to `doc` branch, and triggers centralized documentation update.
- **dash-licenses**: Sets up Java, generates Gradle lockfile (`gradle.lockfile`), downloads Eclipse Dash tool, verifies dependency licenses, and archives compliance report.
- **update-documentation**: Prepares Javadoc via shell script, commits and pushes to `doc` branch, then triggers centralized documentation update.

---

## 📘 Usage Examples

```yaml
name: Publish Snapshot

on:
  push:
    branches: [ main ]

jobs:
  publish:
    uses: eclipse-keyple/keyple-actions/.github/workflows/reusable-publish-snapshot.yml@publish-snapshot-v1
    secrets:
      CENTRAL_SONATYPE_TOKEN_USERNAME: ${{ secrets.CENTRAL_SONATYPE_TOKEN_USERNAME }}
      CENTRAL_SONATYPE_TOKEN_PASSWORD: ${{ secrets.CENTRAL_SONATYPE_TOKEN_PASSWORD }}
      ORG_GITHUB_BOT_TOKEN: ${{ secrets.ORG_GITHUB_BOT_TOKEN }}
```

```yaml
name: Build and Test

on:
  pull_request:

jobs:
  build:
    uses: eclipse-keyple/keyple-actions/.github/workflows/reusable-build-and-test.yml@build-and-test-v1
```

---

## 🤝 Contributing

Please read our [contribution guidelines](https://keyple.org/community/contributing/) before submitting any changes.

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
