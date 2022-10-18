# TOC
[Apply part of a stash](#apply-part-of-a-stash)  
[Stash single file or folder](#stash-single-file-or-folder)  
[Find commits to cherry-pick a feature](#find-commits-to-cherry-pick-a-feature)  
[Check which line ending a file is committed with](#Check-which-line-ending-a-file-is-committed-with)  
[Get of of stuck branch due to line endings](#Get-of-of-stuck-branch-due-to-line-endings)  
[Change the date on a commit](#Change-the-date-on-a-commit)  
[Useful alias'](#useful_alias)  
[Useful git-bash commands](#Useful_git_bash_commands)  
[Only clone one specific sha](#Only-clone-one-specific-sha)

# Apply part of a stash

You've made a stash and wish to checkout only some of the files/folder in that
stash.

```bash
git checkout stash@{0} -- some_dir/myfile.txt
```
for a folder

```bash
git checkout stash@{0} -- my_folder/
```
 
Source <https://riptutorial.com/git/example/7733/apply-part-of-a-stash-with-checkout>

 
# Stash single file or folder

```bash
git stash push file_name

git stash push folder_name
```

# Find commits to cherry-pick a feature

To find all commits on a folder to cherry-pick do

1.  ```git log --reverse --pretty=format:"%h %an %ad% %s" --date=short folder-name ```

    - --reverse reverses the log

    - --pretty=format prints

    -   %h the abbreviated git sha

    -   %an author name

    -   %ad author date

    -   %s the git subject, e.g. the first commit line

    -   --date=short sets the format of the %ad qualifier

    -   There are 4 spaces between each qualifier prints it with 4 spaces,
        it does not look pretty, but it makes it easier to import into
        excel. If you uses the qualifier %x09 it uses a tab instead which
        prints nice e.g.   
        ```git log --reverse --pretty=format:"%h%x09%an%x09%ad%x09%s" --date=short folder_name```

2. Copy the output to a txt file and import it into excel where the
delimiter is 4 spaces.

3. Press the data tab in excel an remove duplicates if you have commits
from several folders

4. Sort by oldest to newest commit


# Check which line ending a file is committed with

```bash
git show HEAD:./file_name | file -
```

# Get of of stuck branch due to line endings
```bash
git branch -f <origin/branch-name> <commit-sha>
git checkout <branch-name>
```
# Change the date on a commit
## Update the last commit to current date

```bash
GIT_COMMITTER_DATE="$(date)" git commit --amend --no-edit --date "$(date)"
```
## Update the last commit to any date date

```bash
GIT_COMMITTER_DATE="Mon Sep 5 12:00 2022 +0000" git commit --amend --no-edit
```

# Revert changes in a specific file for an old commit
1) ```git checkout <commit-sha>~1 path/to/your/file``` the ~1 is important. This checks out the previous commit of the file, before the unwanted changes. The commit-sha must be the commit where the unwanted changes was introduced.
2) ```git restore --staged path/to/your/file```
3) If you need to change anything in the file do it now. I have used it to revert white space changes while keeping the actual wanted change
4) ```git add xxx && git commit```
5) ```git rebase -i HEAD~XXX_back_to_the_commit-sha```
6) Move the new commit to just before commit-sha and squash the new commit into commit-sha
7) Finish the rebase
8) force push to the wanted branch ```git push --force-with-lease```

## Update any commit to any date date
1) ```git rebase <commit-hash>^ -i```
2) select e or edit in the dialog
3) save and quit the dialog
4) ```GIT_COMMITTER_DATE="Mon Sep 5 12:00 2022 +0000" git commit --amend --no-edit```

# Useful alias
## More useful git logs
```lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)'```  

```lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n'' %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all```  

**alias lg to lg1**  
```lg = !"git lg1"```  

## Checkout branch using fzf to search. 
**Only works on local branches**  
```cof = "!checkout_fzf() { git for-each-ref refs/heads/ --format='%(refname:short)' | fzf | xargs git checkout; }; checkout_fzf"```  


# Useful git-bash commands
In your .bashrc file (usually found in the user dir) put the following functions
The following code is from https://polothy.github.io/post/2019-08-19-fzf-git-checkout/
```
# List all branches, sort by latest commit and search with fzf
fzf-git-branch() {
    git rev-parse HEAD > /dev/null 2>&1 || return

    git branch --color=always --all --sort=-committerdate |
        grep -v HEAD |
        fzf --height 50% --ansi --no-multi --preview-window right:65% \
            --preview 'git log -n 50 --color=always --date=short --pretty="format:%C(auto)%cd %h%d %s" $(sed "s/.* //" <<< {})' |
        sed "s/.* //"
}

# checkout a branch using fzf-git-branch to select the branch 
fzf-git-checkout() {
    git rev-parse HEAD > /dev/null 2>&1 || return

    local branch

    branch=$(fzf-git-branch)
    if [[ "$branch" = "" ]]; then
        echo "No branch selected."
        return
    fi

    # If branch name starts with 'remotes/' then it is a remote branch. By
    # using --track and a remote branch name, it is the same as:
    # git checkout -b branchName --track origin/branchName
    if [[ "$branch" = 'remotes/'* ]]; then
        git checkout --track $branch
    else
        git checkout $branch;
    fi
}

# setup alias'
alias gb='fzf-git-branch'
alias gco='fzf-git-checkout'
```


## Only clone one specific sha
Useful when a shallow clone is not enough, especially on a build server when cloning takes a long time.
Uses the fact that a git config can be passed on the fly to git clone command
```
git clone --quiet --config remote.origin.fetch=+${env.GIT_COMMIT_SHA_RESOLVED}:refs/remotes/origin/${env.GIT_COMMIT_SHA_RESOLVED} --config advice.detachedHead=false https://GITREPO.git --no-checkout --depth 1
pushd ${env.repoName}
git checkout ${env.GIT_COMMIT_SHA_RESOLVED}
popd
```
