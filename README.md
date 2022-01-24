# `cloud-nuke` automation

This repository contains the GitHub Actions workflow and configuration files that execute 
[`cloud-nuke`](https://github.com/gruntwork-io/cloud-nuke) for cleaning up AWS accounts.

# Table of Contents

- [Usage](#usage)
- [Links](#links)

## Usage

This repository implements a very simple GitHub Actions workflow defined under `.github/workflows/cloud-nuke.yaml`. The
workflow uses the `.github/workflows/contexts.json` file to determine the list of AWS accounts that need to be cleaned
up, and YAML configuration files stored in the `config` directory for instructing `cloud-nuke` what resources need to
be included or excluded from the cleanup operation.

At a high level, the workflow performs the following actions:

1. Builds a list of AWS accounts that will be cleaned up.
2. Downloads a specific version of the  `cloud-nuke` executable using the version tag specified in the 
   `CLOUD_NUKE_VERSION` environment variable.
   1. This is done to avoid using the `latest` version - new versions of the tool often add new types of resources to be
      nuked, and that could result in unexpected behaviour if we don't actually want those specific resources gone.
3. Executes `cloud-nuke` against all configured AWS accounts.

To add a new AWS account to the cleanup workflow, follow these steps:

- [Create a new environment in GitHub](https://docs.github.com/en/actions/reference/environments#creating-an-environment).
  - The environment name must follow the naming pattern of `account_name-account_id` - e.g. `sandbox-123456789012`.
  - It is recommended to enable an [environment protection rule](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches)
    that restricts deployments to the environment to protected branches only. Because `cloud-nuke` is a very destructive
    tool, allowing deployments from all branches could result in incidents where incorrect and unreviewed configuration 
    in a branch causes unexpected deletion of AWS resources.
- Append the new environment's configuration to `.github/workflows/contexts.json`, e.g.:
```json
{
  "account_name": "sandbox",
  "account_id": "123456789012"
}
```

The workflow runs on a schedule which is defined in `.github/workflows/cloud-nuke.yaml`.

## Links

- [`cloud-nuke` documentation](https://github.com/gruntwork-io/cloud-nuke).
