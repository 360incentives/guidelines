# Git Workflow

## Historical Branches

`master` - used for production. Must stay stable at all time. Contains the release history.

`develop` - used for staging. Contains the development edge.

`fastlane` - used for fastlane. Contains the development edge for an epic.

## Development branches
All development work should be done in a dedicated branch. This encapsulation 
makes it easy for multiple developers to work on a particular feature without 
disturbing the main codebase. It also means the `master` branch will never 
contain build braking code.

## Workflow
### Development
When ready to start development branch out from `master`

```
Master  *-*-*
             \
Feature       \-*-*-*-*
```

### Code Review
Once you have finished the development a pull request is need to merge into `develop`

### Acceptance
The development branch is merged into `develop`, which should trigger the deployment
from circleci

```
Master      ----*
                 \
Feature           \-*-*-*-*
                           \
Development ----------------m
```

### Deployment to Production
Deployments to productions are completed by merging `develop` to `master`

```
Master      ----*---------------------------m
                 \                         /
Feature1          \-*-*-*-*               /
                   \       \             /
Feature2            \-*-*-*-\-*-*       /
                     \       \   \     /
Bug                   \-*-*   \   \   /
                           \   \   \ /
Development ----------------m---m---m
```

### Reverting a feature
If a feature is not ready to be released it need to be reverted from development.
This is done be reverting the merge in `develop`.

```bash
$ git revert -m 1 <SHA> # where <SHA> is the SHA of the merge
```

`develop` should be merged into `master` following the normal workflow. Once 
`master` has been deployed to production it should be merged into the feature 
development. To fix the feature now you will now need to **revert the revert**.
This revert should be done in the feature branch.

```bash
$ git revert <SHA> # where <SHA> is the SHA of the revert
```

```
Master      ----*------------------------------m
                 \                            / \
Feature1          \-*-*-*-*------------------/---m-R-*
                   \       \                /         \
Feature2            \-*-*-*-\-*-*          /           \
                     \       \   \        /             \
Bug                   \-*-*   \   \      /               \
                           \   \   \    /                 \
Development ----------------m---m---m--r-------------------m
```
