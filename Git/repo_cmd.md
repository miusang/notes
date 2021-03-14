# repo命令

* 退回到指定日期  
  注意：这个日期是指commit日期，不是merge日期。

  ```
  repo forall -c 'commitID=`git log --before "2017-03-17 0700" -1 --pretty=format:"%H"`;git reset --hard $commitID'
  参数说明：
  forall 操作分支所有的仓库
  -c 只操作当前分支
  --before 早于指定时间点的提交记录
  -1 只显示最近的1条记录
  "2017-0317 07:00" 回退的日期
  --pretty 以指定格式显示提交记录
  %H 提交记录的hash值，及commit id
  ```

  

