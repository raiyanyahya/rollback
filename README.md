# ⏮ rollback

A github action to manually rollback the master branch to the previous commit, creating a timestamped backup branch for recovery.

## ⚠ Warning: 

This action force pushes to the master branch, which can cause issues for other collaborators. Use with caution and ensure you understand the implications of removing a commit before proceeding.
This action is for developers and teams who want and immediate `rollback` of their master branch. Running this action will change master and `rewrite` the commit history. Changes will be lost `forever`.
Be very `careful` and `sure` whe you use this. 

## Why

Its so common that we mess up or do a bad commit / merge on the master branch and we want to rollback production. It causes panic and frustration. Using this action anyone in the team who has permissions on the repo can perform a rollback of the master branch. It can save time and be very handy when under a lot of stress. 

## How it works

1. The action checks if it's running as a workflow dispatch event. If not, it exits with an error.
2. The action authenticates with GitHub using the provided token.
3. The repository is cloned, and the action checks if the current branch is master. If not, it exits with an error.
4. A new backup branch with a timestamp in its name is created and pushed to the remote repository.
5. The master branch is rolled back to the previous commit, and the updated master branch is force pushed to the remote repository.
6. The action cleans up the temporary repository directory.

## Limitations and Potential Issues

1. Force pushing: Force pushing to the master branch is generally discouraged, as it can cause confusion and issues for other collaborators.
2. Loss of history: Since the action erases the history of the current commit, you may lose important changes or context if you do not carefully review the backup branch before using this action.
3. Limited to the master branch: This action is hardcoded to work only on the master branch. You may need to modify the action to support different branch names or make the branch name configurable.
4. Permission issues: The action requires a GitHub token with sufficient permissions to clone the repository, create and push branches, and force push to the master branch. If the token does not have the required permissions, the action will fail.

## Usage
To use this action, create a new workflow in your repository at .github/workflows/manual_rollback.yml with the following content:

```
name: Manual Rollback

on:
  workflow_dispatch:

jobs:
  rollback:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Use Manual Rollback Action
      uses: raiyanyahya/rollback@master
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
```

## Contributing

Contributions to this project are welcome! Please feel free to open issues or submit pull requests with bug fixes, improvements, or new features.

## License

This project is licensed under the MIT License.

