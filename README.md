[![Carbonetes](https://cdn.carbonetes.com/carbonetes-plugin/assets/branding/branding_header.png)](https://carbonetes.com)

# Azure DevOps Plugin: Carbonetes Comprehensive Analysis

## Introduction

**Carbonetes** provides comprehensive container analysis and policy evaluation as a fully managed service. Carbonetes analyzes your container images for native code vulnerabilities, software composition analysis (SCA), license types, malware, secrets, and bill of materials.

Upon committing your code, the Carbonetes automatically initiates a comprehensive container analysis scan. The results of that scan are compared to the applicable policy to determine whether the container should build or not. The insight from the analysis and the policy evaluation are embedded right inside Azure DevOps Pipelines making it easy to find and resolve issues without ever leaving Azure DevOps Pipelines.

To know more about Carbonetes, check [our website](https://carbonetes.com).

## Task Usage

This task seamlessly integrates comprehensive container analysis directly into your CI/CD pipeline.

### Docker image scan example

```yaml
- task: CarbonetesComprehensiveAnalysis@1
  inputs:
    registryUri: '12345678910.dkr.ecr.us-west-2.amazonaws.com'
    repoImageTag: 'busybox:latest'
    username: 'myusername@carbonetes.com'
    password: 'mypassword'
```

## Prerequisites

This pipe requires a valid **Carbonetes credentials** `(email and password)`.

- Doesn't have any credentials yet? [Register now](https://console.carbonetes.com/register).

## Inputs Description

| Input Name                  | Description                                                  |
| --------------------------- | ------------------------------------------------------------ |
| registryUri \*              | Registry Uri (Added in Carbonetes Web Application) |
| repoImageTag \*             | The image to be scan. |
| username \*                 | The account username on Carbonetes. |
| password \*                 | The account password on Carbonetes. |
| failOnPolicy                | Decides if it builds or stops the build based on the policy evaluation. Default: `true`. |

_\* = required inputs._

*Note : to be aligned in your CI/CD pipeline, make sure that you supply the same REPOIMAGETAG that has been built within your pipeline stages. See below sample pipeline stages.* 

![Pipeline Stages](/images/sample-pipeline-stages.png)

## Output Description

| Output Name                  | Description                                                                                  |
| ---------------------------- | -------------------------------------------------------------------------------------------- |
| Vulnerabilities              | A list of known security risks that can be exploited by a threat actor listed with severities. |
| Software Compositions        | Software that might cause a security risk listed with severities. |
| Software Dependencies        | Pieces of software that rely on each other listed with vulnerability counts. |
| Licenses                     | Legal compliance found on each software of the scanned image. |
| Malware                      | Virus threats found on the scanned image. |
| Secrets                      | Secret data found on each software of the scanned image. |
| Bill of Materials            | A list of all the components exist in a software. |
| Policy Result                | The result of the policy evaluation, `PASSED` or `FAILED`. |
| Final Action                 | Decide if the build will `STOP` or `GO` based on the policy evaluation. |

## Complete CI/CD Example

```yaml
trigger: 
- master

pool:
  vmImage: "ubuntu-latest"

stages:
  - stage: Build and Push
    jobs:
      - job:
        steps:
        - script: docker build -t busybox:latest .

        - task: CarbonetesComprehensiveAnalysis@1
          inputs:
            registryUri: '12345678910.dkr.ecr.us-west-2.amazonaws.com'
            repoImageTag: 'busybox:latest'
            username: 'myusername@carbonetes.com'
            password: 'mypassword'
            # Note: to be aligned in your CI/CD pipeline,
            #       make sure that you supply the same repoImageTag
            #       that has been built within your pipeline stages.
        
        - script: docker push busybox:latest
```

## Support
To help with this task extension, or have an issue or feature request, please contact: [eng@carbonetes.com](eng@carbonetes.com)

If reporting an issue, please include:

* the version of the task extension
* relevant logs and error messages
* steps to reproduce

## License and Copyright

Copyright © 2021 Carbonetes

Licensed under [MIT License](LICENSE).