# megalinter-azure-pipelines-with-github-repository

This repository demonstrates how to set up MegaLinter in an Azure DevOps pipeline while using GitHub as the code source with status reporting in the github pull requests. The default configuration produces an artefact with the report and can fail the pipeline if there are issues, but it requires effort to fetch the report and view it.  
The pipeline configuration (`azure-pipelines.yaml`) adds 3 additional environment variables to support GitHub integration. Remember to set `GITHUB_COMMENT_REPORTER` to `true` and `AZURE_COMMENT_REPORTER` to `false` in the `.mega-linter.yml` configuration. The parameters were deduced from [GithubStatusReporter.py](https://github.com/oxsecurity/megalinter/blob/main/megalinter/reporters/GithubStatusReporter.py)

## Background

The company I worked at used bitbucket and didn't have pipelines. When choosing the appropriate tooling for an Azure-native project, I decided to go with GitHub for source, as the most common choice, but Azure DevOps pipelines over GitHub actions, as I considered it could be very well integrated with Azure resources for deployment. I am pretty content with it so far. In rare cases such as this, I had to spend a bit more time on the setup though.

## Parameters

Compared to the base MegaLinter setup, this pipeline introduces three additional environment variables to support GitHub integration:

1. **GITHUB_TOKEN**: Used to authenticate with GitHub for API access and report publishing. You have to create this token in [personal-access-tokens](https://github.com/settings/personal-access-tokens)
2. **GITHUB_REPOSITORY**: Specifies the GitHub repository name (e.g., `owner/repo`) to associate linting results and actions.
3. **GITHUB_SHA**: Provides the commit SHA from the GitHub repository from the job context.

GitHub access token needs the taget repository access and the following permissions:

1. Read access to code and metadata
2. Read and Write access to commit statuses, discussions, and pull requests

After creating the token (usually with temporary lifetime), create a variable group and add it in the [Variable groups](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/variable-groups?view=azure-devops&tabs=azure-pipelines-ui%2Cyaml)