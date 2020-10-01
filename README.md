# MLOPS AUTOMATION


### MLOPS = MACHINE LEARNING on DEVOPS


# Project on integration of Machine Learning  with Jenkins & Docker 

This project is based on the the automation of machine lerning model , when it'll be integrated with docker & jenkins. Bascially to create a machine learning model  is not a tough task as we've to run it  , or trained it & find the accuracy all these things we can find easily with the automation process. we have to only define the job in jenkins according to the our requirements & use cases.and you can make jenkins purely automatic by using the plugin of pipeline's.


# Feature of Project

  1. Create container image that’s has Python3 and Keras or numpy  installed  using dockerfile 

  2. When we launch this image, it should automatically starts train the model in the container.

  3. Create a job chain of job1, job2, job3, job4 and job5 using build pipeline plugin in Jenkins 

  4.  Job1 : Pull  the Github repo automatically when some developers push repo to Github.

  5.  Job2 : We've to the build the our **own docker image** according the our code ( that docker image has been created by me though the **Dockerfile** inside that we've to write the all the libraries required for that code ).

  6.  Job3 : We've to launch the container in docker according to our CNN code.

  7.  Job4 : We,ve to run the CNN code inside our docker & find the **Accuracy** of the model.

  8.  Job5 : We,ve to send the notification to the developer if accuracy of the model is more than 80%.


# DESCRIPTION OF PROJECT

firstly i write the python code for CNNN model in jupyter notebook & after that i download that code in to **.py extension**  & then copy this code into a seperate folder or directory inside the **RHEL8** operating system with the help of **Winscp software**. After that i upload this code on **GitHub** where i created a repository for the same & initialize this repository in githu


After that i launch my **RHEL8** operating systemtcreate the seperate **workspace** to work on this project & & create A docker file with command 
```
vim Dockerfile  

```
inside this file i used centos: latest version as my base image & install all the required libraries to run the CNN code .

![](MLOps-automation/Dockerfile.png)

######  CODE For Dockerfile:

```

FROM centos:latest

RUN yum install python36 -y 

RUN pip install --upgrade pip 

From tensorflow/tensorflow:latest

RUN pip install numpy

RUN pip install scikit-learn

RUN pip imnstall matplotlib

RUN pip imnstall pandas

RUN pip imnstall seaborn

RUN pip installl keras

RUN pip imnstall pillow

FROM MNIST-CNN.py

```

After that i start jenkins where i create the chain of job to run the all the processs automatically.

  - Job1 : Pull  the Github repo automatically when some developers push repo to Github.
  
  ![](MLOps-automation/job1-1.png)
  
  ![](MLOps-automation/job1-2.png)
  
  ![](MLOps-automation/job1-3.png)
  
  ```
  sudo cp -v -r -f * /root/mlopsws/
  
  ```
  
  
  ![](MLOps-automation/job1-output.png)

  - Job2 : We've to the build the our **own docker image** according the our code ( that docker image has been created by me though the **Dockerfile** inside that we've to write the all the libraries required for that code ).
  
  
   ![](MLOps-automation/job2-1.png)
   
   ![](MLOps-automation/job2-2.png)
   
   ```
   sudo docker build -t mlops_lib:v1  /root/mlopsws/
   if sudo docker ps -a | grep mlops
   then docker rm -f  $(sudo docker ps -aq)
   else
   sudo docker run -dit --name mlops mlops_lib:v1
   fi 
   
   ```
   
  ![](MLOps-automation/job2-output1.png)
   
  ![](MLOps-automation/job2-output2.png)
   
   
  - Job3 : We've to launch the container in docker according to our CNN code.
 
   ![](MLOps-automation/job3-1.png)
  
   ![](MLOps-automation/job3-2.png)
  
  ```
  if sudo docker ps |grep mlops
  then 
  echo "container already runing "
  else 
  sudo docker ps -dit -v /root/mlopsws/  --name mlops1 mlops_ws:v1
  fi 
  ```

  ![](MLOps-automation/job3-output.png)

 -  Job4 : We,ve to run the CNN code inside our docker & find the **Accuracy** of the model.
 
  ![](MLOps-automation/job4-1.png)
  
  ![](MLOps-automation/job4-2.png)
  
  
  ``` 
  sudo docker exec mlops1 python3 MNIST-CNN.py
  ```
   
![](MLOps-automation/job4-output.png)
   
![](MLOps-automation/job4-output2.png)
    
![](MLOps-automation/job4-output3.png)
     
 -  Job5 : We,ve to send the notification to the developer if accuracy of the model is more than 80%
 
  to send the mail to developer i write a code in python language for sending the mail
  
  
![](MLOps-automation/mail-code.png)
  
 
 ```
 
 #mail.py
import smtplib 

# creates SMTP session 
s = smtplib.SMTP('smtp.gmail.com', 587) 

# start TLS for security 
s.starttls()   

# Authentication 
s.login("sender's mail id", "sender's mail id password")
 
# message to be sent 
message = "Hey Developer, Finally we got the model trained. "

# sending the mail 

s.sendmail("sender's mail id", "reciever's mail id", message)
# terminating the session 

s.quit()

```
![](MLOps-automation/job5-1.png)
  
![](MLOps-automation/job5-2.png)
   
![](MLOps-automation/job5-3.png)
  
```
sudo python3 /root/mlopsws/mail-code.py
```
![](MLOps-automation/job5-output.png)
  
![](MLOps-automation/email-verification.png)

# pipeline view of job 

![](MLOps-automation/build-pipelineview.png)
