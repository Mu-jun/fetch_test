# Question
## 1. 로컬과 레포의 커밋이력이 달라졌을 때  fetch의 효용성
- 일단 레포의 커밋이력을 로컬 git에서 확인 가능하게 해주는 것을 확인
- push 또는 pull 은 안됨.
```
# git push error message
error: failed to push some refs to 'https://github.com/Mu-jun/fetch_test.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
```
# git pull error message
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
```
- 대충 설정을 통해 pull할 때 자동으로 rebase 또는 merge 하도록 설정하는 방법 알려주는 듯.
## 2. ```git pull -f``` 시도
- 다행히 덮어씌어지지 않고 오류 메시지를 보여준다.
```
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```
- fatal: Need to specify how to reconcile divergent branches. << 이 부분만 추가되었다.
## 3. ```git help pull``` 확인
- 깃 설정을 하면 편하지만 나중에 잘 기억하지 못해서 도움말에서 해결방법 찾아봄.
```
       --ff, --no-ff
           When merging rather than rebasing, specifies how a merge is handled
           when the merged-in history is already a descendant of the current
           history. If merging is requested, --ff is the default unless
           merging an annotated (and possibly signed) tag that is not stored
           in its natural place in the refs/tags/ hierarchy, in which case
           --no-ff is assumed.

           With --ff, when possible resolve the merge as a fast-forward (only
           update the branch pointer to match the merged branch; do not create
           a merge commit). When not possible (when the merged-in history is
           not a descendant of the current history), create a merge commit.

           With --no-ff, create a merge commit in all cases, even when the
           merge could instead be resolved as a fast-forward.
```
- fast-forward 설정으로 pull 하는 방법
```
       -r, --rebase[=false|true|merges|interactive]
           When true, rebase the current branch on top of the upstream branch
           after fetching. If there is a remote-tracking branch corresponding
           to the upstream branch and the upstream branch was rebased since
           last fetched, the rebase uses that information to avoid rebasing
           non-local changes.

           When set to merges, rebase using git rebase --rebase-merges so that
           the local merge commits are included in the rebase (see git-
           rebase(1) for details).

           When false, merge the upstream branch into the current branch.

           When interactive, enable the interactive mode of rebase.

           See pull.rebase, branch.<name>.rebase and branch.autoSetupRebase in
           git-config(1) if you want to make git pull always use --rebase
           instead of merging.

               Note
               This is a potentially dangerous mode of operation. It rewrites
               history, which does not bode well when you published that
               history already. Do not use this option unless you have read
               git-rebase(1) carefully.
```
- rebase 설정으로 pull 하는 방법
```
       --squash, --no-squash
           Produce the working tree and index state as if a real merge
           happened (except for the merge information), but do not actually
           make a commit, move the HEAD, or record $GIT_DIR/MERGE_HEAD (to
           cause the next git commit command to create a merge commit). This
           allows you to create a single commit on top of the current branch
           whose effect is the same as merging another branch (or more in case
           of an octopus).

           With --no-squash perform the merge and commit the result. This
           option can be used to override --squash.

           With --squash, --commit is not allowed, and will fail.

           Only useful when merging.
```
- merge 설정으로 pull 하는 방법?
```
       -f, --force
           When git fetch is used with <src>:<dst> refspec it may refuse to
           update the local branch as discussed in the <refspec> part of the
           git-fetch(1) documentation. This option overrides that check.
```
- 이전에 시도했던 강제로 pull
- 도움말을 읽어보니 fetch를 먼저 시도해서 덮어씌어지지 않은 걸지도?

## 4. ```git pull -r``` 시도
- pull 되었고 conflict가 발생해서 해결하라고 메시지 나옴.
```
% git pull -r

remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 655 bytes | 327.00 KiB/s, done.
From https://github.com/Mu-jun/fetch_test
   47c3a9a..f11b7aa  main       -> origin/main
Auto-merging git pull -r conflict test
CONFLICT (content): Merge conflict in git pull -r conflict test
error: could not apply 9bbdba0... local working
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 9bbdba0... local working
```

# Todo
```
1. --squash, --no-squash
2. fetch를 먼저 하지 않고 git pull -f 바로 시도 
```