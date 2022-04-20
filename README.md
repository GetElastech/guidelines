# guidelines
Project coding guidelines

When working on a feature:
1. Create a feature branch in the GetElastech fork repo https://github.com/GetElastech
2. Do smaller bite-sized PRs into that dev branch. These can be reviewed by Elastech team + DL team members as needed, and merged into the feature branch.
3. Rebase this branch regularly on top of the upstream master, add commits for any conflicts
4. Submit large feature branch pull request upstream to onflow/flow-go repo, and Dapper will do code review of final branch.
5. The size of each feature is less important. What we’re interested in is making sure that what’s merged into master on onflow/flow-go is complete and usable by the flow community.
6. We use codesubmit@getelastech.com to work within the GetElastech fork, and use this account to create the PR against onflow/flow-go

This is an example commit message

```
commit 255e00168f9984492651658826a349e95823ab53 (HEAD -> feature/observer-prc-0420c)
Author: GetElastech <codesubmit@getelastech.com>
Date:   Wed Apr 20 21:09:21 2022 +0000

    [2064] Configure gRPC API to run on Observer Service
    Authors: Miklos Szegedi
```

Update command for default email and name:

```
git config --global --edit
```

Content:
```
[user]
        name = GetElastech
        email = codesubmit@getelastech.com
```
