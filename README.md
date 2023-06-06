# trunk-based-development
A demo of Trunk Based Development (TBD).

> **NOTES:**
* This example assumes you're using GitHub. If you're using GitLab or some other Git-based host then you'll need to make the necessary adjustments to the commands provided.
* The provided commands also include an example of the output. This is to give you some idea of what *should* happen when you issue the command. Be aware, however, that your output might not match exactly.
* Some of the examples use Python. You don't need to have Python installed to follow along, however. You can replace the examples written in Python with whatever you'd like.
* The command examples include a typical bash shell prompt (`$`). Do not include this if you intend to copy/paste to follow along.

## Clone the Repository
The first thing we need to do is clone the repository.

If you're using SSH (highly recommended) then the command will be something like this:

```shell
$ git clone git@github.com:<org>/<repository>.git
```

If you're using HTTPS then it will look something like this:

```shell
$ git clone https://github.com/<org>/<repository>.git

For example:

```shell
$ git clone git@github.com:gregmajor/trunk-based-development.git
Cloning into 'trunk-based-development'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), done.
```

> **NOTE:** The following examples assume that the trunk is named `main` which is the default for GitHub at this time. Older repositories often use `master`. Truthfully, however, the name of the trunk is irrelevant and can be whatever you want.

## Create a Feature
For smaller teams (just a few developers), working directly off of `main` is pretty common. However, when scaling TBD the recommendation is to adopt *short-lived* branches created from the trunk. It is also a great idea to adopt a clear, consistent naming strategy for these branches. A very common approach is something like this:

`<issue-type>/<issue-id>`

Where:

* `<issue-type>` is an accepted identifier for the type of work. For example, `feature`, `bug`, `chore`, and so forth.
* `<issue-id>` is the unique identifier assigned by the work management system (e.g., Jira, Trello, ServiceNow).

For example:

`feature/DM-14223`

> **NOTE:** Jira users might consider taking advantage of the handy shortcut provided to create branches based on the ticket ID and name. This results in a branch name similar to this:

`feature/DM-14223-update-user-account-page`

Be aware, however, that you will still need to add the `feature/` prefix manually!

For our example, we'll assume we're working on a new feature for ticket `GM-123`. Now we're ready to create our branch:

```shell
$ git checkout -b feature/GM-123
Switched to a new branch 'feature/GM-123'
```

At this point you can make whatever changes you'd like. Let's create a simple Python script to satisfy our new feature:

```shell
$ cat << EOF > tbd.py
print("This is my new feature!")
EOF
```

Let's test our feature to make sure it works like we expect:

```shell
$ python tbd.py
This is my new feature!
```

Now let's stage our new file:

```shell
git add tbd.py
```

Now create a commit:

```shell
$ git commit -m "Added the tbd Python script"
[feature/GM-123 709ae70] Added the tbd Python script
 1 file changed, 1 insertion(+)
 create mode 100644 tbd.py
```

Finally, let's push our changes to GitHub:

```shell
$ git push origin feature/GM-123
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 10 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 354 bytes | 354.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'feature/GM-123' on GitHub by visiting:
remote:      https://github.com/gregmajor/trunk-based-development/pull/new/feature/GM-123
remote:
To github.com:gregmajor/trunk-based-development.git
 * [new branch]      feature/GM-123 -> feature/GM-123
```

Usually we'd run unit tests locally before we staged our changes and *certainly* before we push our code. For this example, we'll assume that running the script and seeing the expected output is sufficient.

Finally, we want our changes to be merged to the trunk. To signal our intent we'll create a Pull Request. Since Pull Requests are not a feature of Git, you'll need to create a new Pull Request in GitHub either on the website or via the GitHub CLI.

At this point several things will happen in a mature organization including:

* The build pipeline will build the code and execute the unit tests. If the build fails or the unit tests don't pass then the Pull Request will *not* be created and the submitter will be notified.
* Code quality tools (e.g., linters, GitGuardian, Codacy, etc.) will run checks. If the checks do not pass then the Pull Request will not be created and the submitter will be notified.
* One or more team members will manually review the code. If they see something they want changed or have questions then they can make comments on the Pull Request and ideally communicate directly with the submitter.
* Once the PR is merged, the trunk will be built and deployed to the appropriate environments.

## Creating Another Feature
We're moving right along! Let's create a new feature. The first thing we need to do is make sure that our local trunk branch is up to date with the remote trunk branch (`main` in our example). To do that, we first need to make sure we've checked out the trunk:

```shell
$ git checkout main
M	.gitignore
M	README.md
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

Notice that the file we created (`tbd.py`) isn't there. That's because we need to update our local copy of the trunk branch. To do that we'll run this command:

```shell
$ git pull --rebase origin main
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (1/1), 640 bytes | 640.00 KiB/s, done.
From github.com:gregmajor/trunk-based-development
 * branch            main       -> FETCH_HEAD
   326953d..3d2ec2e  main       -> origin/main
Successfully rebased and updated refs/heads/main.
```

Now our `tbd.py` file should be present along with all the other changes that have happened. Let's proceed with our second feature by creating a new short-lived feature branch assuming issue `GM-456` and then editing our `tbd.py` file:

```shell
$ git checkout -b feature/GM-456
Switched to a new branch 'feature/GM-456'
```

```shell
$ cat << EOF > tbd.py
print("This is my new feature!")
print("This is my second new feature!")
EOF
```

After we test, we can stage the change, commit, and push just like before:

```shell
$ git add tbd.py
```

```shell
$ git commit -m "Adding another awesome feature"
[feature/GM-456 46d33ee] Adding another awesome feature
 1 file changed, 1 insertion(+)
 ```

 ```shell
 $ git push origin feature/GM-456
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 10 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 2.08 KiB | 2.08 MiB/s, done.
Total 7 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
remote:
remote: Create a pull request for 'feature/GM-456' on GitHub by visiting:
remote:      https://github.com/gregmajor/trunk-based-development/pull/new/feature/GM-456
remote:
To github.com:gregmajor/trunk-based-development.git
 * [new branch]      feature/GM-456 -> feature/GM-456
 ```

Finally, just like before, we can create a Pull Request to get our new feature merged to trunk.

## Rebasing

> **NOTE:** For the purposes of this demo, we're going to simulate a merge conflict by editing the `tbd.py` file directly in GitHub on the trunk.

Uh-oh! Looks like somebody made changes and now our code can't be automatically merged. No big deal. Let's get everything squared away.

First, let's update our local trunk (`main` in our case):

```shell
$ git checkout main
M	README.md
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)
```

```shell
$ git pull --rebase origin main
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 748 bytes | 249.00 KiB/s, done.
From github.com:gregmajor/trunk-based-development
 * branch            main       -> FETCH_HEAD
   3d2ec2e..ff6386e  main       -> origin/main
Successfully rebased and updated refs/heads/main.
```

Now we can get the latest commits in trunk and incorporate them into our feature branch:

```shell
$ git checkout feature/GM-456
Switched to branch 'feature/GM-456'
```

```shell
$ git rebase main
Auto-merging tbd.py
CONFLICT (content): Merge conflict in tbd.py
error: could not apply 46d33ee... Adding another awesome feature
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 46d33ee... Adding another awesome feature
```

Oh no! The dreaded merge conflict! Not to worry, use your favorite method to resolve the conflict then follow the supplied instructions. For example:

```shell
$ git mergetool
Merging:
tbd.py

Normal merge conflict for 'tbd.py':
  {local}: modified file
  {remote}: modified file
```

> **NOTE:** The built-in diff and merge capabilities for Git *will* work, but it's highly recommended that you install a three-way merge tool and configure Git to use it.

