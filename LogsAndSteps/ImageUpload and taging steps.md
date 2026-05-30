<-Install Docker->

root@master:~/mydocker# curl -fsSL https://get.docker.com -o install-docker.sh
root@master:~/mydocker# sudo sh install-docker.sh
<creating doker file>

root@master:~/mydocker# vi Dockerfile

          FROM tomcat:10.1-jdk21
          MAINTAINER OMKAR
          COPY webappk8sdemo-V_01.war /usr/local/tomcat/webapps/
          EXPOSE 8080
          CMD ["catalina.sh","run"]


<Building docker image>

root@master:~/mydocker# docker build -t myjavaapp:1 .

<Taging docker image>

#Fist Need to Login
root@master:~/mydocker# docker login

USING WEB-BASED LOGIN

i Info → To sign in with credentials on the command line, use 'docker login -u <username>'


Your one-time device confirmation code is: CBHS-FZDB
Press ENTER to open your browser or submit your device code here: https://login.docker.com/activate

Waiting for authentication in the browser…

WARNING! Your credentials are stored unencrypted in '/root/.docker/config.json'.
Configure a credential helper to remove this warning. See
https://docs.docker.com/go/credential-store/

Login Succeeded

# now need to tag image
root@master:~/mydocker# docker images

IMAGE         ID             DISK USAGE   CONTENT SIZE   EXTRA
myjavaapp:1   a2ef4ca68dde        755MB          245MB

root@master:~/mydocker# docker tag myjavaapp:1 omkarsinghaws/myjavaapp:1
root@master:~/mydocker# docker push omkarsinghaws/myjavaapp:1


The push refers to repository [docker.io/omkarsinghaws/myjavaapp]

510ce46061a4: Pushed
1: digest: sha256:a2ef4ca68dde0ed129bf8c15063a90187a3c38c206074d1dfa6e4e1caebf810a size: 856
root@master:~/mydocker#

#Creating docker file for second image

            FROM tomcat:10.1-jdk21
            MAINTAINER OMKAR
            ADD  https://aws-s3-k8s-bucket.s3.ap-south-1.amazonaws.com/webappk8sdemo_V_02.war /usr/local/tomcat/webapps/
            EXPOSE 8080
            CMD ["catalina.sh","run"]

#Building docker image 2
root@master:~/mydocker# docker build -t myjavaapp:2 .

#taging and pushing image 

root@master:~/mydocker# docker tag myjavaapp:2 omkarsinghaws/myjavaapp:2
root@master:~/mydocker# docker push omkarsinghaws/myjavaapp:2

#Listing images
#root@master:~/mydocker# docker images

IMAGE                       ID             DISK USAGE   CONTENT SIZE   EXTRA
myjavaapp:1                 a2ef4ca68dde        755MB          245MB
myjavaapp:2                 3fb27af6e36e        755MB          245MB
omkarsinghaws/myjavaapp:1   a2ef4ca68dde        755MB          245MB
omkarsinghaws/myjavaapp:2   3fb27af6e36e        755MB          245MB
root@master:~/mydocker#
