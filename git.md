# TOC
[Apply part of a stash](#apply-part-of-a-stash)  
[Stash single file or folder](#stash-single-file-or-folder)  
[Find commits to cherry-pick a feature](#find-commits-to-cherry-pick-a-feature)  
[Check which line ending a file is committed with](#Check-which-line-ending-a-file-is-committed-with)  
[Get of of stuck branch due to line endings](#Get-of-of-stuck-branch-due-to-line-endings)

# Apply part of a stash

You've made a stash and wish to checkout only some of the files/folder in that
stash.

```bash
git checkout stash@{0} -- some_dir/myfile.txt
```
for a folder

```bash
git checkout stash@{0} -- algo/
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
