
more about Raghav - https://automationstepbystep.com/
Today we will learn :
1. What is Dockerfile
2. How to create Dockerfile
3. How to build image from Dockerfile
4. Basic Commands

TIPS & TRICKS

Dockerfile : 
A text file with instructions to build image
Automation of Docker Image Creation

FROM
RUN
CMD

Step 1 : Create a file named Dockerfile

Step 2 : Add instructions in Dockerfile

Step 3 : Build dockerfile to create image

Step 4 : Run image to create container

COMMANDS
: docker build 
: docker build -t ImageName:Tag directoryOfDocekrfile

: docker run image


https://www.youtube.com/watch?v=LQjaJINkQXY

Run get executed during the building of the
CMD runs only when we create container out of the image


Run docker build -t imagename:tagname "location of dockerfile"

Run image to create container
Docker run imageId

https://github.com/wsargent/docker-cheat-sheet#dockerfile

