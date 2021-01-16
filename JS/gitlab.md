pipeline:
connected sequential steps. (output of one process is input of others)
<<<<<<< HEAD
jobs are independent from one another, they dont exchange the data if not told to
the env where job1 executed destroys as soon as finishes the job
we need to tell the job which is our output/artifact

in order to tell our job what to save, we need to define artifact
    artifacts:
      paths:
        -build/

add ls or cat anytime to debug the file
```
stages:
    - build
    - test

build the car:
    stage: build
    script:
        - mkdir build
        - cd build
        - touch car.txt
        - echo "chassis" >> car.txt
        - echo "engine" >> car.txt
        - echo "wheels" >> car.txt
    artifacts:
        paths:
            - build/

test the car:
    stage: test
    script:
        - ls
        - test -f build/car.txt
        - cd build
        - cat car.txt
        - grep "chassis" car.txt
```


The newly created build foler will not be available in repo. they are created in pipeline and they are saved independent of git project as artifacts.

### gitlab Architecture

gitlab server: allows to create repo and save in a server
assoon as we create a pipeline, its managed by server as well as its also delegated to **gitlab runner** (make sure artifacts are saved, run the pipeline etc) 
at minimum we have one gitlab runner, but based on our requirement we can add as many runner as we want(scaledup/down)
=======



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
>>>>>>> eb31086469c6d752948dde63c993af90f85b260a

