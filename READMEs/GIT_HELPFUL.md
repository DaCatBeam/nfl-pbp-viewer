# Helpful Git commands

## Basics
There are 3 primary stages your code can be in, in terms of git
  * None staged - Git is aware of changes but they have not been staged for commit
  * Staged - Git has staged the code changes for a file, meaning they are ready to be added to a commit
  * Committed - Git has saved the code under a hash, and it is now part of the commit tree.

#### Creating a new branch
`$ git checkout -b <new_branch_name>`

#### Adding Code to "Staging"
`$ git add <file_path or folder>`

#### Commiting Code
`$ git commit -m 'Commit Message'`

#### Pushing commit to corresponding remote branch
`$ git push origin <branch_name>`

#### Checking commit history
`$ git log`

#### Rebasing onto different branch (dev as an example)
`$ git rebase -i dev`

#### Rebasing onto branch in upstream repository (dev as an example)
`$ git rebase -i upstream/dev`
* Note upstream must be set up through `$ git remote add upstream <repo git@github>`

#### Rebasing on current branch (To alter commit history)
```bash
# SIDENOTE: Check your editor w/:
$ echo $EDITOR

# If you get nothing, you've probably got vi as a default.
# If it is vi, you'll hit 'i' to insert data
# Then the ESC button, type ':x' and hit enter
# Or it might be 'ctlr + x', then 'y' and hit enter

# Check git log
$ git log

# Lets say we get this as output:
commit de3b780a8deecadbb3255de0a483ea275366029f (HEAD -> NFLPBP-2)
Author: Ben R <dacatbeamgithub@gmail.com>
Date:   Fri Sep 4 23:57:24 2020 -0500

    Commit I want to 'squash' into 'NFLPBP-2 READMEs'

commit 450aee93b1fb10f7bb2acf3670936c9e6bd4fa4d (origin/NFLPBP-2)
Author: Ben R <dacatbeamgithub@gmail.com>
Date:   Fri Sep 4 23:10:04 2020 -0500

    NFLPBP-2 READMEs

commit 39e4c33a3de6069a47dc73664c703121ba665a8f (origin/master, origin/dev, origin/HEAD, dev)
Author: DaCatBeam <65837050+DaCatBeam@users.noreply.github.com>
Date:   Wed Aug 26 19:48:17 2020 -0500

    Initial commit

# To Add these commits together I would run
$ git rebase -i HEAD~2 # 2 being the number of commits we want to rebase

# At this point the editor will come up with:
pick 450aee9 NFLPBP-2 READMEs
pick de3b780 Commit I want to 'squash' into 'NFLPBP-2 READMEs'

# Rebase 39e4c33..de3b780 onto 39e4c33 (2 commands)
# ...Other commented out stuff

# To combine these two and keep the commit messages I would:
pick 450aee9 NFLPBP-2 READMEs
squash de3b780 Commit I want to 'squash' into 'NFLPBP-2 READMEs'

# To combine these two and drop the 2nd commit message, I would:
pick 450aee9 NFLPBP-2 READMEs
fixup de3b780 Commit I want to 'squash' into 'NFLPBP-2 READMEs'

# After the changes I would see:
$ git log

commit 540382f4ad25d15d313cfdef0b366a01e54a3cb6 (HEAD -> NFLPBP-2)
Author: Ben R <dacatbeamgithub@gmail.com>
Date:   Fri Sep 4 23:10:04 2020 -0500

    NFLPBP-2 READMEs

commit 39e4c33a3de6069a47dc73664c703121ba665a8f (origin/master, origin/dev, origin/HEAD, dev, NFLPBP-3)
Author: DaCatBeam <65837050+DaCatBeam@users.noreply.github.com>
Date:   Wed Aug 26 19:48:17 2020 -0500

    Initial commit

# Where the changes from the original 2nd and 3rd commit are combined into the 2nd commit.

# Verify you're trying to push to the right branch w/
$ git push origin <branch_name>

# This should come out with an expected rejection like:
 ! [rejected]        NFLPBP-2 -> NFLPBP-2 (non-fast-forward)
error: failed to push some refs to 'git@github.com:DaCatBeam/nfl-pbp-viewer.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.


# To rewrite the remote history, we'll need to force-push to remote w/
$ git push origin <branch_name> -f
```

## Stashing
#### Stashing Code changes w/o staging or committing
`$ git stash`

* This will push the changes into a stack for retrieval

#### Removing from stash
Get/Pop the top stash entry
* `$ git stash pop`

Apply code in stash entry but leave copy of changes in the stack
* `$ git stash apply`

List stash contents
* `$ git stash list`

Pop particular entry (Note, might need to escape the brackets)
* `$ git stash pop stash@{<entry_number>}`
