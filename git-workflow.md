# Git Workflow

## Stable Branches
- `master` - used for production. Must stay stable at all time. Contains the production release history.
- `develop` - used for staging. Contains the development edge.
- `fastlane` - used for fastlane. Contains the development edge for an epic.

## Development branches
All development work should be done in a dedicated branch. This 
makes it easy for multiple developers to work on a particular feature without 
disturbing the main codebase. It also means the `master` branch will never 
contain build breaking code. 

## Workflow
### Development
When you're ready to start development branch out from `master`

```
git checkout master
git pull
git checkout -b feature/#1234-strong-passwords
```

```
master  *-*-*
             \
feature/#n    \-*-*-*-*
```

### Code Review
Once you have finished a feature, open a PR to merge into `develop`.

### Acceptance
The development branch is merged into `develop`, which should trigger a deployment
on CircleCI.

```
master      ----*
                 \
feature/#n        \-*-*-*-*
                           \
develop     ----------------m
```

### Production
Deployments to production are completed by creating a release branch from master and
successively merging the selected\* feature branches into the release branch. This allows features
to be 'cherry-picked' into production without having to wait for all features in develop
to receive Product Owner approval. 

\**Waffle is currently used to to mark issues by adding them to the "Deploying" column. Both relsr and flightplan use the
waffle labels to identify issues for the next release.*

```
master          ---------*-------------------m-------
                   \      \                 /
release/20171231-101500    \----m-------m--*
                     \         /       /
feature/#n            \-*-*-*-*       / 
                       \       \     / 
feature/#x              \-*-*-*-\-*-* 
                         \       \   \ 
bug/#b                    \-*-*   \   \ 
                               \   \   \ 
develop     --------------------m---m---m----------
```

To create the release branch and PR you can either use [relsr](https://github.com/jcleary/relsr) on the command-line or allow [flightplan](https://flightplan.createl.io) to create
them for you on the deployment schedule (currently PR at 10:15, merge at 10:30 daily).

### Naming convention
Use the following naming convention:
- Features : `feature/#1234-number-feature-title`
- Bug: `bug/#1234-bug-title`
- Technical: `technical/#1234-technical-issue-title`
- Grouped Issues: `group/#1234-group-title`
- Rework: `rework/1234-issue-title-rework-1` <= note lack of # in branch name
- Release: `release/yyyymmdd-hhmmss` (auto created by relsr / flightplan)

### Resolving Conflicts
If GitHub warns you of conflicts in your PR, resolve the conflicts by merging your branch into develop on the command line. 
Do not use the GitHub tools or the command line to merge `develop` into your branch as this will create deployment dependencies. 
The process looks like this:
```bash
git checkout develop
git pull
git merge feature/#1234-strong-passwords
# resolve conflicts
git commit
git push
```

When you push develop, GitHub will close the PR for you automatically. 

### Rework
1. Pull the original branch
1. Create a rework branch without the # (e.g rework/1234-fix-breaking-test-part-1)
1. When ready, open the PR back to the original branch - this will ensure the PR is just for the most recent changes.
1. After the PR is merged, manually merge the feature branch back into `develop` to get your changes on to Staging.
1. If there is subsequent rework, repeat the above steps, creating a new rework branch (e.g. rework/1234-fix-breaking-test-part-2)

### Special Workflow (Grouping Issues)
From time to time it may become necessary to deploy certain features together. In these cases please ensure you follow the special workflow:

1. Create a issue for combining the features. (e.g. Issue 5678 - Languages and React Group Deployment), referencing the original issues in the description.
1. Create a branch for the issue ( e.g. `group/#5678-languages-and-react` )
1. Merge all the feature branches into the group branch.
1. Close the original issues.
1. When you're ready to deploy the entire group, you will be able to create a PR for the group branch to `master`
1. If rework is required on any of the issues in the group, use the Rework process above for the group issue (branch out of and PR back into the `group/#` branch)

