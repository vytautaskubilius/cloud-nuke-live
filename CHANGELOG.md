# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## 2022-01-25

### Added

- [Renovate Bot](https://docs.renovatebot.com) for keeping `cloud-nuke` up to date.

### Changed

- The `cloud-nuke-dry-run` job now only runs on `push` events, while the remaining jobs only run on `schedule` events.
  - This is done to avoid doing a dry-run before every scheduled job as there is no benefit to doing that.

## 2022-01-24

### Changed

- The `cloud-nuke-dry-run` job is now run on all branches (including `main`) to make sure consistent testing and a more
  straightforward workflow.

## 2022-01-20

### Added

- Configuration for the `sandbox` AWS account.
