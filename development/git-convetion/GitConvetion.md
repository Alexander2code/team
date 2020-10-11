Git Practices

## Git practices

### General recommendations

* To avoid inconsistencies in CI/CD tool, configure your Git settings to use exactly the same Name (\<First Name\> \<Last Name\>) and Email as you use in CI/CD tool. Never use personal accounts to commit changes.
* Don't commit directly to the `master` branch. If you use Gitflow development approach don't commit to `development`/`release` branches directly. For each feature or bugfix create an individual branch (see recommendations on branch names below) and create PR (see recommendations on PRs below) when the work is finished.
* Commit every day. Commit multiple times a day as there are logical checkpoints. Don't wait until the entire feature is done. If you work alone in the branch you could even commit unfinished broken code just to not occasionally miss it.
* Write meaningful comments (see more below). Don't write just 'bugfix', 'refactoring', etc. It's impossible to support such repositories in the future.
* Don't merge your branches manually - use Pull Requests. See more on PRs below.
* All commit messages must be in English.

### Why commit and push often

* Saving your work at the server frequently helps you to not lose accidentaly the changes (because of accidental file deletion, hardware failure or various human factors).
* Saving your work at the server helps you to share the progress with your teammates.
* If you commit frequently it will solve the problem of huge commits with a lot of changes not related to each other. Try to commit by small logical chunks to help others to understand your changes.
* Frequent commits will trigger CI which can find issues you don't see on your local PC.

### Try to avoid when creating commits ([reference](https://wiki.openstack.org/wiki/GitCommitMessages#Things_to_avoid_when_creating_commits))

* Mixing whitespace changes with functional code changes. Create 2 commits, one with the whitespace changes, one with the functional changes.
* Mixing two unrelated functional changes in the scope of one commit.
* Try to split renaming/moving files and changing their content into two commits. Otherwise, Git doesn't understand that the files have been renamed and detects it as deletion/addition.
* Sending large new features in a single giant commit. If a code change can be split into a sequence of patches/commits, then it should be split.

### Commit messages

[Here is the article](https://chris.beams.io/posts/git-commit/) with seven rules of [a great Git commit message](https://commit.style/). The list of these rules (read explanation by the link above):

1. Separate subject from body with a blank line.
2. Limit the subject line to 50 characters. (is not a hard limit, just a rule of thumb from OSS projects, but consider 100 characters as the hard limit).
3. Capitalize the subject line.
4. Do not end the subject line with a period.
5. Use the imperative mood in the subject line ('Fix ...' instead of 'Fixed ...' or 'Fixes ...').
6. Wrap the body at 72 characters.
7. Use the body to explain what and why vs. how.

Meaningful commit subject is required for all commits. But as for body messages, try to use your best judgment - if you see that the body could help others to understand your changes better, please write it.

If the commit refers to an issue, add this information to the commit message header or body.

When writing a commit message there are some important things to remember (read more [here](https://wiki.openstack.org/wiki/GitCommitMessages#Information_in_commit_messages)):

* Do not assume the reviewer understands what the original problem was.
* Do not assume the reviewer has access to external web services/site.
* Do not assume the code is self-evident/self-documenting.
* Describe why a change is being made.
* Read the commit message to see if it hints at improved code structure.
* The first commit line is the most important.
* Describe any limitations of the current code.

### Branch names

Clear branch names allow us to understand the type of the changes (new feature or just bugfix), the content of the changes and the story it implements.

* Use `release/`, `feature/`, `fix/` or `bug/` prefixes (or any other you agree on at your team) to group your branches. Sometimes it's useful to put developer name to the branches to additionally group, for example, `feature/alexandr2code/<ticket_number>-feature-name`. If it works for your team, feel free to use it.
* Include ticket numbers to the branch name to be able to read more information about the changes of the branch. Even if you included ticket number to the branch name please write 1-5 words about the changes to help others understand the basic idea of the branch without any need to open a ticket system.
* Split words in a branch name by a dash sign ('-').

## Merge from `master` / `development` to feature branch

There are cases you need to pull changes from a 'trunk' branch to your feature branch. It may be due to the need to resolve merge conflicts before PR, or to actualize a branch during long development. 

If so, use "rebase" instead of "merge". It will keep the history cleaner.

CMD:

1. git rebase origin/master
2. git push --force (if needed)

SourceTree:

1. Press "Pull" button
2. Change "Remote branch to pull" to "master"
3. Check "Rebase instead of merge" checkbox
4. Press "Pull" button

## Pull Requests practices

### General recommendations

* Provide meaningful descriptions to every PR on what features implemented. Include Jira/DevOps ticket numbers which were resolved. Please preserve default CI/CD tool prefix for all PRs messages "Merged PR XXXX:".
* Every PR must be reviewed by at least one other developer. The exception could be made only in case of fixing an urgent Production issue. But even in this case, it's better to show your changes to colleagues.
* Always rotate reviewers withing the team. It will allow sharing the knowledge with other team members. For small teams occasionally ask to review architects or peers from other teams. Seniors can (and even must) be reviewed by less experienced developers.
* Provide feedback during the review via comments. Even if you discussed the review verbally, you have to write down the comments. In the future, it could help to understand why you decided to implement the feature in this exact way.
* Please provide your feedback as soon as possible to not block others work.
* By default, please use English language for all comment messages. But taking into account that not everybody knows English well, it's better to provide a clear description in Russian than a useless message in English. On the other side, writing even not perfect comments in English improves your language skills. So please agree on what language to use with your team.

### Pull Request merge types

Here are four merge type allowed in CI/CD tool:

1. Squash commit. Creates a linear history by condensing the source branch commits into a single new commit on the target branch.

Please consider this merge type as the default one for merging feature branches. Try to have your feature branches as short as possible. If you have several commits in the branch, quite often most of them are just fixes after the review process and are not valuable to be put into the `master` / `development` branch. If your branch has several logical commits you want to merge to the target branch in order to not miss related messages, then use the 'Merge (no fast-forward)' type. But if possible try to rebase your source branch to squash small commits with fixes into a commit with logical change.

2. Merge (no fast-forward). Preserves nonlinear history exactly as it happened during development.

Please consider this merge type as the default one for merging `development` branches into `master` one as we don't want to miss history. As written above, use this merge type for feature branches only in case if you splitted the feature into multiple logical commits and want to save history.

3. Rebase and fast-forward. Creates a linear history by replaying the source branch commits onto the target without a merge commit.

Please don't use this type of merges to not mess up the `master` branch history with all commits from your source branch. Use 'Merge (no fast-forward)' instead.

4. Semi-linear merge. Creates a semi-linear history by replaying the source branch commits onto the target and then creating a merge commit.

Please don't use this type of merges to not mess up the `master` branch history with all commits from your source branch. Use 'Merge (no fast-forward)' instead.

## Exceptions

You could ignore the recommendations in the following cases (but please approve this with your team):

1. In case you change binary files only, for example, images, certificates, public keys, etc. In this case it could be useless to review PRs as you could not even compare the files. Although sometimes it's better to create PR with detailed description on what was done to document the decisions.

2. In case you change files for absolutely legacy system which is not supported for years, nobody in your team knows what it is and could not help you with review. And if you don't want to spend time of your team members on researching/reviewing the system they will never support.

> Thanks for reading this.
