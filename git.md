# Git

---
# Git SVN Crash Course

- <http://git.or.cz/course/svn.html>
- svn/cvs存储diff信息，git存储快照

---
# Git原理图

![Git](http://nettedfish.sinaapp.com/images/3.git_architecture.png)

---
# Git原理图(2)

![Git2](http://www.softel.co.jp/blogs/tech/wordpress/wp-content/uploads/2011/08/git-arrows311.png)

---
# Git命令详解

![GitCmd](http://nettedfish.sinaapp.com/images/7.git_tree.png)

---
# git commit & git commit --amend

git commit --amend --reset-author

![GitCommit](http://nettedfish.sinaapp.com/images/9.git_commit.png)

---
# git checkout

git checkout maint

![GitCheckout](http://nettedfish.sinaapp.com/images/10.git_checkout.png)

---
# git reset

git reset HEAD~3

![GitReset](http://nettedfish.sinaapp.com/images/11.git_reset.png)

---
# git diff

![GitDiff](http://nettedfish.sinaapp.com/images/8.git_diff.png)

---
# git merge

git merge other

![GitMerge](http://nettedfish.sinaapp.com/images/12.git_merge.png)

---
# git rebase

git rebase master

![GitRebase](http://nettedfish.sinaapp.com/images/14.git_rebase.png)

---
# git cherry-pick -- Bug Fix

git cherry-pick 2c33a

![GitCherry](http://nettedfish.sinaapp.com/images/13.git_cherry-pick.png)

---
# git cheat sheet

<img src="http://byte.kde.org/~zrusin/git/git-cheat-sheet-medium.png" width="800" height="500" />

---
# git stash

- git stash
- git stash pop
- git stash list
- git stash save
- git stash list

---
# git bisect


	In a C project I was working on recently, one of our regression tests began failing. The project was in an in-between state, so I figured something was just temporarily broken, and as we filled things back in it would probably pass again.

    A good number of commits went by and things started to come together, but this old test was still failing. Clearly someone had actually introduced a bug along the line, rather than merely introducing a temporary hole in functionality.

    So I run git bisect start, then git bisect bad on the experimental head of the repository. Then I jump back about thirty commits to find one where the test passes and I run git bisect good. git then jumps me to a commit halfway between the known good and the known bad commits, I run the test and do git bisect good or git bisect bad. Repeat this process about five times and I'm right at the commit where the bug was introduced.

    I'd done a fairly innocent seeming cast of a pointer, which screwed up some pointer arithmetic, since you're so curious.

    All in all it took just a couple of minutes. However, it turns out I did this the slow way. Since I had a test program that returned 0 on success and something else on failure, I could have simply given git the command to run it, and it could have found the commit in question in seconds. See: git bisect run
 
- git bisect start
- git bisect bad/good
- git bisect run (自动找到有引入bug的分支)

---
# git clean -f

- 删除任何不在index的文件

---
# git grep

    If on a git repo, use `git grep`. Similar options, much, much faster.

    Example, on a directory with 11K+ files:

    grep -r . 'float: none' -exclude-dir=.git
    56.96s user 0.70s system 99% cpu 58.127 total

    git grep 'float: none'
    0.42s user 1.05s system 335% cpu 0.438 total

    130x+ improvement :D

---
# git blame

---
# hooks

.git/hooks/pre-commit

    !bash
    git diff
    for FILE in `git diff-index --name-status HEAD -- | cut -c3-` ; do
        # Check if the file contains 'println'
        if [ `grep 'println' $FILE` ]
        then
            echo $FILE ' contains println!'
            exit 1
        fi
    done
    exit

---
# git lg

    !bash
    git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --"
    
    git lg

![GitLg](http://coolshell.cn//wp-content/uploads/2012/06/git.log_.02.png)

---
# custom git command

git mail --mailto chris,tailor

cat ~/bin/git-mail

    !python
    import gflags
    import sys
    import os

    FLAGS = gflags.FLAGS
    gflags.DEFINE_list('mailto', '', 'Comma separated reviewers, will add @abc.com if not exists', short_name = 'm')
    gflags.DEFINE_list('cc', '', 'Comma separated cc-list, will add @abc.com if not exists')
    gflags.DEFINE_string('githost', 'origin', 'Git remote host')
    gflags.DEFINE_string('branch', 'master', 'Git branch')

    def add_abc_domain(mail_list):
        l = []
        for i in mail_list:
            if not "@" in i:
                l.append(i + "@abc.com")
            else:
                l.append(i)
        return l

    def generate_cmd(mailto_list, cc_list):
        cmd = "git push --receive-pack='git receive-pack "
        for i in mailto_list:
            cmd += "--reviewer=%s " % i
        for i in cc_list:
            cmd += "--cc=%s " % i

        cmd += "' %s HEAD:refs/for/%s" % (FLAGS.githost, FLAGS.branch)
        return cmd


    def main():
        try:
            argv = FLAGS(sys.argv)[1:]  # parse flags
        except gflags.FlagsError, e:
            print '%s\\nUsage: %s ARGS\\n%s' % (e, sys.argv[0], FLAGS)
            sys.exit(1)

        mailto_list = add_abc_domain(FLAGS.mailto)
        cc_list = add_abc_domain(FLAGS.cc)

        cmd = generate_cmd(mailto_list, cc_list)

        os.system(cmd)

    if __name__ == "__main__":
        main()
    
---
# git-removebranch (危险操作！)

git removebranch

    !bash
    git branch | awk '$1!="*" && $1!="master" {print $1}' | xargs git branch -d

---
# gitstats

- git history statistics generator <https://github.com/hoxu/gitstats>

---
# tig

- Text-mode interface for git <https://github.com/jonas/tig>
![tig](http://jonas.nitro.dk/tig/screenshots/blame-view.png)

---
# Git tools

- Git for windows <http://msysgit.github.io>
- Tortoise Git <https://code.google.com/p/tortoisegit/>
- Free Git GUI for mac & windows <http://www.sourcetreeapp.com>

---
# Gerrit

Android Source Code <https://android-review.googlesource.com/#/q/status:open,n,z>

<img src="http://www.worldhello.net/wpfiles/2010/11/gerrit-workflow.png" width="800" height="550" />

---
# Multiple git repositories

- repo <http://source.android.com/source/developing.html>
- git submodule <http://josephj.com/entry.php?id=342>
- git subtree <http://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/>

---
# Git Deliver

- Delivery system based on git push and ssh <https://github.com/arnoo/git-deliver>

---
# Gitric

- Simple git-based deployment for fabric <https://github.com/dbravender/gitric>

---
# References

- Git Magic <http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/>
- 图解Git <http://nettedfish.sinaapp.com/blog/2013/08/05/deep-into-git-with-diagrams/>
- A Git Primer <http://danielmiessler.com/study/git/>
- Git flow 開發流程 <http://ihower.tw/blog/archives/5140>
- Git对象细节 <http://www.gitguys.com/topics/the-git-object-model-starting-with-the-blob/>
- Git对象细节 <http://schacon.github.io/gitbook/1_the_git_object_model.html>
- Import from subversion <https://help.github.com/articles/importing-from-subversion>
- Svn to Git Migration <http://subgit.com>
- 我的书签 <http://pinboard.in/u:fakechris/t:git>

---
# Gitlab流程

- 在group下创建工程，提交基础版本
- (A)fork
- (A)git clone git@192.168.0.245:xxx/yyy.git
- (A)git remote add upstream git@192.168.0.245:xxx-origin/yyy.git
- (A)git checkout -b yourbranch
- (A)git commit -a
- (A)git push -u origin remotebranchname
- (A)open merge request
- (B)review accept merge request/comment/merge manually
- (A)git fetch upstream
- (A)git merge upstream/master

---
# Have Fun!

    !bash
    gource -1280x720 --seconds-per-day 2 --hide filenames,dirnames --colour-images --camera-mode track --highlight-users
