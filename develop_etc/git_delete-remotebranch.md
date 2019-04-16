## Git RemoteBranch 삭제하기


git에서 로컬에서 작업할 임시 브랜치가 잘못해서 리모트로 올라간 경우 삭제하는 방법!



```
//첫번째 방법
git branch origin --delete [branchName]
```

```
//두번째 방법
git branch -d [branchName]
git push origin [branchName]
```


