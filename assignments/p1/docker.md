---
layout: default
title: Docker integration
nav: assignments
---

[**Back to Lab 1**]({{ site.url }}/assignments/lab1.html)

## Remote development using containers

This article would help people who used IDEs a lot and got used to Remote development, autocomplete and other features. However, Remote deployment and debugging for C is not an easy way to setup, therefore we have prepared some scripts and suggestions to setup your dev environment.

**Disclaimer 1** The code might contain bugs and the offered solution might have unknown issues in your environment, please use provided ideas at your own risk, as it might slow down your development process.

**Disclaimer 2** Even though it should work the same on Windows, none of us have tested it on this platform. Please discuss issues on Piazza.

**Note**
If you like to develop in CLI, containers might be an easy way to go.

#### Install steps

1. Install **Docker* on your OS
2. Install **CLion IDE** [**https://www.jetbrains.com/clion/download/**](https://www.jetbrains.com/clion/download/) and get a student license: [**https://www.jetbrains.com/student/**](https://www.jetbrains.com/student/).
3. Build a container from Docker image. You can just download an image from Dockerhub: `docker pull ebagdasa/cs5450_p1` or here is the *Dockerfile*:

    ~~~ docker
    FROM ubuntu:16.04

    RUN apt-get update && apt-get install -y openssh-server cmake gcc build-essential
    RUN apt-get install vim python tcpdump telnet -y
    RUN apt-get install byacc flex -y
    RUN apt-get install iproute2 gdbserver less bison valgrind -y

    RUN mkdir /var/run/sshd
    RUN echo 'root:root' | chpasswd
    RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

    # SSH login fix. Otherwise user is kicked off after login
    RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

    ENV NOTVISIBLE "in users profile"
    RUN echo "export VISIBLE=now" >> /etc/profile

    EXPOSE 22 9999 7777
    CMD ["/usr/sbin/sshd", "-D"]
    ~~~

   We expose 3 ports: 22, 9999 and 7777.
   * Port 22 will be used to SSH into the container,
   * Port 9999 can be used to connect to the program from outside
   * Port 7777 is used to run `gdbserver` program that allows to debug the program remotely.

4. Start the container:
    ~~~ bash
   docker run -d -p 3022:22 -p 7777:7777 -p 9999:9999 -h server.cornell.edu  \
      --security-opt seccomp:unconfined  \
      --name server ebagdasa/cs5450_p1:latest
   ~~~

   This option `--security-opt seccomp:unconfined` is required to allow remote debugger to run.

   If you want to run sever and client containers you can start them both and then communicate through Docker network (`172.17.0.0/16`), but in this project it is redundant.

6. Try to SSH into your container `ssh -p 3022 root@127.0.0.1`. The password is `root`. You can copy your SSH public key from your machine.

6. Copy your code to the server. You can setup Remote Deployment in CLion to automatically upload code. Here is the [**link to configuration**](https://www.jetbrains.com/help/clion/2016.1/creating-a-remote-server-configuration.html)

6. Please refer to this [**link**](https://blog.jetbrains.com/clion/2016/07/clion-2016-2-eap-remote-gdb-debug/) and this [**link**](https://blog.jetbrains.com/clion/2016/11/clion-2016-3-eap-remote-gdb-debug-udl-rename-and-code-analysis-fixes/) to setup CLion Remote Debugging. Note, for MacOS you would have to configure your local GDB with x86 linux target.
This is the sample config on MacOS:
![image]({{ site.url }}/assignments/p1/my_config.png)

7. Obtain symbol file. Symbol file is essentially a compiled program with debug information: `gcc -g -o example example.c`. You need to compile it in your container, then copy to your machine and specify in CLion Run Configuration.

8.  To automate this process you can use the **script.sh** inside your container:

    ~~~ bash
    #!/usr/bin/env bash

    OUTPUT="$(ps -ef | grep gdbserver | awk '/echo_server/ { print $2}')"
    echo "gdbserver PID: $OUTPUT"
    kill -9 "$OUTPUT"
    OUTPUT="$(ps -aux | awk '/echo_server/' | awk 'NR==1{print $2}')"
    echo "the running program $OUTPUT"
    kill -9 "$OUTPUT"
    sleep 5
    make clean
    make
    gcc -g -o server echo_server.c
    gdbserver 127.0.0.1:7777 ./echo_server &
    ~~~

9. After the script run **server** file back to laptop `scp -P 3022 root@127.0.0.1:/root/code/starter_code/echo_server /$YOUR_LOCATION/starter_code/`

10. Set breakpoints in CLion and run the Remote Debugger.

### Troubleshooting

1. Always check that ports are open:

    ~~~ bash
    docker ps -a
    CONTAINER ID        ***  PORTS
    a89893c7c0d9        ***  0.0.0.0:7777->7777/tcp,
                             0.0.0.0:9999->9999/tcp,
                             0.0.0.0:3022->22/tcp
    ~~~

2. Don't forget to copy symbol file
3. Don't forget to put breakpoints in the reachable code.


*If you have problems to get it working, please ask them on Piazza.*
