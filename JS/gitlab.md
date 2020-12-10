pipeline:
connected sequential steps. (output of one process is input of others)



Runners are processes that pick up and execute jobs for GitLab
Gitlab CI will automatically assign jobs to the "test" stage, even if no test stage was defined.
The Gitlab Runner is the tool responsible for actually running the commands defined in a job


Why gitlab CI?
- simple scalable architecture
- Docker first approach
- Pipeline as code
- Merge request with CI support

### basic CI/CD workflow

- create a simple website
- Automatically build a new version
- Automatically deploy the newest version


### integrate docker
image: node

### Why do jobs fail
After a command is executed it will return an exit status
0 => Job succeeded
1- 255 => ERROR: Job failed: exit code 1

`grep "gitLab" ./JS/gitlab.md` 
`echo $?` // 0

