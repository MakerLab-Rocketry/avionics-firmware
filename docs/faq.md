# FAQ

Frequently asked questions. If your question isn't here, ask the team and add the answer once it's resolved.

## General

**What is this repository for?**
It holds the firmware for our EuRoC rocket avionics: the flight computer, the DAQ board, shared drivers, tests and Python tools.

**Where do I start?**
Read [onboarding.md](onboarding.md).

## Setup

**Which tools do I need?**
See the [tools list in onboarding](onboarding.md#tools-to-install).

<!--
## Building and flashing

**How do I build the firmware?**

TODO

**How do I flash a board?**

TODO
-->

## STM32CubeMX

**I regenerated code from the `.ioc` file and my changes disappeared. Why?**
CubeMX overwrites generated files. Only code inside `/* USER CODE BEGIN */ ... /* USER CODE END */` blocks is preserved. Put your own code in `App/` or `shared/` instead.

## Drivers

**Where should a new driver go?**
In `shared/` if both boards may use it (header in `shared/inc/`, source in `shared/src/`), otherwise in that board's `App/` folder.

**How should I name and structure a driver?**
Follow the [driver development guidelines](driver-guidelines.md).

**How do I test a driver?**
Add tests to `tests/` and test it on real hardware before merging. Note the results in the pull request.

## Git and pull requests

**Can I push directly to `main`?**
You can try but branch protection rules prohibit it.
Create a branch, open a pull request and get a review.

**How do I link a pull request to an issue?**
Write `Closes #<issue number>` in the PR description. The issue closes automatically when the PR is merged and the remote branch is deleted automatically.

**My PR was merged. How do I clean up branches on my machine?**
The remote branch is deleted on GitHub automatically, but your machine still keeps a copy of it until you clean up:

```sh
git switch main
git pull --ff-only
git fetch --prune                # remove references to branches deleted on GitHub
git branch -d feature/tmp102-driver   # delete your local branch
```

If `git branch -d` refuses because the branch is "not fully merged" (this happens with squash or rebase merges), check that the PR really was merged on GitHub and then use `git branch -D <branch>`.

> **Tip**: run `git config --global fetch.prune true` once, and every `git fetch` / `git pull` will prune automatically.

## Tools

**What is in `tools/`?**
Python utilities, such as decoders for logged and telemetry data.
