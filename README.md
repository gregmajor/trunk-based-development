# trunk-based-development
A demo of Trunk Based Development (TBD).

> **NOTE:** This example assumes you're using GitHub. If you're using GitLab or some other Git-based host then you'll need to make the necessary adjustments to the commands provided.

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
cat << EOF > tbd.py
print("This is my new feature!")
EOF
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

