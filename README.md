# Info

You can find some utils scripts that can be used to improve your workflow using
gitflow and redmine.
On redmine each issue have a specific subject composed by story code and story
name. The story code is composed from project code and story number, eg.:

```
AB-123 - Story name: first item specification
CD-35 - Other story: other item specification
```

Using gitflow in our process development each issue is processed in a different
branch. The branch name is composed from story code, issue number and issue
subject, eg.:

```
AB-123_9593_story_name_first_item_specification
CD-35_6053_other_story_other_item_specification
```

# Install

You need to install [python-redmine](https://github.com/maxtepkeev/python-redmine):

```
easy_install python-redmine
```

Then clone the current repository:

```
git clone https://github.com/mavimo/git-redmine-utilities.git
```

And copy the ```git-redmine``` script on your bin folder:

```
cp git-redmine /usr/bin/git-redmine
```

# Configuration

Start your configuration coping the ```.redmine.cfg.dist``` file in your home
directory or in the git folder and rename it to ```.redmine.cfg``` eg:

```
cp .redmine.cfg.dist ~/.redmine.cfg
```
and edit the ```~/.redmine.cfg``` file:

```
[Redmine]
hostname=
username=
password=

[Pattern]
branch=%(story_prefix)-%(story_code)_%(issue_id)_%(story_name)
message=refs #%(issue_id): %(story_prefix)-%(story_code) - %(story_name):
```

configuring the redmine ```hostname``` (no trailing slash), ```username``` and
```password```. You can also customize ```branch``` and ```message``` pattern.

You shuld also copy the ```prepare-commit-msg``` in ```.git/hooks``` folder.

# Usage

Init git in the project folder and gitflow:

```
mkdir project-folder
cd $_
git init
git flow init
```

then copy the ```prepare-commit-msg``` as described on _configuration_ section.

Now we can start a new feature using the appropriate pattern on branch creation
with the specified pattern with:

```
git redmine [ISSUE_ID]
```
eg:

```
~/project-folder (develop) $ git redmine 6053

Switched to a new branch 'feature/CD-35_6053_other_story_other_item_specification'

Summary of actions:
- A new branch 'feature/CD-35_6053_other_story_other_item_specification' was created, based on 'develop'
- You are now on branch 'feature/CD-35_6053_other_story_other_item_specification'

Now, start committing on your feature. When done, use:

     git flow feature finish CD-35_6053_other_story_other_item_specification
```
Edit and add file on staging area, when you need to commit you can just write:
```
git commit
```
and the script automatically generate the commit message in accord with pattern
specified on configuration, then you can improve the commit message:
```
refs #6053: CD-35 - Other story: other item specification:
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch feature/CD-35_6053_other_story_other_item_specification
# Changes to be committed:
#       new file:   test.txt
```
