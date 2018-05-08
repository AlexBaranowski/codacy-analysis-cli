# Codacy Analysis CLI

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/e490e1a232a04bccb113ff55b8126947)](https://www.codacy.com?utm_source=git@bitbucket.org&amp;utm_medium=referral&amp;utm_content=qamine/codacy-analysis-cli&amp;utm_campaign=Badge_Grade)
[![Codacy Badge](https://api.codacy.com/project/badge/Coverage/e490e1a232a04bccb113ff55b8126947)](https://www.codacy.com?utm_source=git@bitbucket.org&utm_medium=referral&utm_content=qamine/codacy-analysis-cli&utm_campaign=Badge_Coverage)

Small command line interface to execute Codacy code analysis locally.

## Features

- [P] Invoke a tool
  - [D] Local tool configuration file
  - [D] Remote Codacy patterns, ignored files and language extensions
  - [ ] Default settings
- [P] Invoke multiple tools
- [ ] Invoke tools in parallel
- [P] Post results to Codacy
- [ ] Exit with status from Codacy quality settings

> [D] - Done | [P] - Partially Done | [ ] - Not Started

## Prerequisites

* Java 8+
* SBT 1.1.x
* Scala 2.12.x

## Build

### Compile

* **Code**

    **Note:** - Scapegoat runs during compile in Test, to disable it, set `NO_SCAPEGOAT`.

        sbt compile
        
* **Tests**

        sbt test:compile

### Test

```sh
sbt test
```

### Format Code

```sh
sbt scalafmtCheck
sbt scalafmt
```

### Dependency Updates

```sh
sbt dependencyUpdates
```

### Static Analysis

```sh
sbt scapegoat
sbt scalafix sbtfix
```

### Coverage

```sh
sbt coverage test
sbt coverageReport
sbt coverageAggregate
export CODACY_PROJECT_TOKEN="<TOKEN>"
sbt codacyCoverage
```

### Docker

* **Local**

        sbt 'set version := "<VERSION>"' docker:publishLocal

* **Release**

        sbt 'set version := "<VERSION>"' docker:publish

## Install

### MacOS

```bash
brew tap codacy/tap
brew install codacy-analysis-cli
```

### Others

```bash
curl -L https://github.com/codacy/codacy-analysis-cli/archive/master.tar.gz | tar xvz
cd codacy-analysis-cli-* && sudo make install
```

## Usage

### Local

```sh
sbt "runMain com.codacy.analysis.cli.Main analyse --tool <TOOL-SHORT-NAME> --directory <SOURCE-CODE-PATH>"
```

### Docker

```sh
docker run \
  --rm=true \
  --env CODACY_CODE="$CODACY_CODE" \
  --volume /var/run/docker.sock:/var/run/docker.sock \
  --volume "$CODACY_CODE":"$CODACY_CODE" \
  --volume /tmp:/tmp \
  codacy/codacy-analysis-cli \
    --tool <TOOL-SHORT-NAME>
```

### Script

```sh
codacy-analysis-cli analyse \
  --tool <TOOL-SHORT-NAME> \
  --directory <SOURCE-CODE-PATH>
```

## Configuration

### CLI Parameters

* `--tool` - Choose the tool to analyse the code (e.g. brakeman)
* `--directory` - Choose the directory to be analysed
* `--codacy-api-base-url` or env.`CODACY_API_BASE_URL` - Change the Codacy installation API URL to retrieve the configuration (e.g. Enterprise installation)
* `--output` - Send the output results to a file
* `--format` - Change the output format (e.g. json)
* `--commit-uuid` - Set the commit UUID that will receive the results on Codacy
* ` --upload` - Request to push results to Codacy 

### Local configuration

To perform certain advanced configurations, Codacy allows to create a configuration file.
Check our [documentation](https://support.codacy.com/hc/en-us/articles/115002130625-Codacy-Configuration-File) for
more details.

### Remote configuration

To run locally the same analysis that Codacy does in your code you can request remotely the configuration.

#### Project Token

You can find the project token in:
* `Project -> Settings -> Integrations -> Add Integration -> Project API`

```sh
codacy-analysis-cli analyse \
  --project-token <PROJECT-TOKEN> \
  --tool <TOOL-SHORT-NAME> \
  --directory <SOURCE-CODE-PATH>
```

> In alternative to setting `--project-token` you can define CODACY_PROJECT_TOKEN in the environment.

#### API Token

You can find the project token in:
* `Account -> API Tokens`

The username and project name can be retrieved from the URL in Codacy.

```sh
codacy-analysis-cli analyse \
  --api-token <PROJECT-TOKEN> \
  --username <USERNAME> \
  --project <PROJECT-NAME> \
  --tool <TOOL-SHORT-NAME> \
  --directory <SOURCE-CODE-PATH>
```

> In alternative to setting `--api-token` you can define CODACY_API_TOKEN in the environment.
