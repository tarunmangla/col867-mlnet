# Assignment 2 - Active Measurements using NetUnicorn Platform

In the class, we discussed about NetUnicorn, a platform for running
reproducible active measurements. The goal of this assignment is to get our
hands dirty with it. 

## Part 1: Installing netUnicorn

NetUnicorn supports running measurements on different kinds of infrastructure
such as Amazon AWS instances, Raspberry Pi, and Docker container. In this
assignment, you will work with a local environment using docker container. 

Your first task is to deploy a netUnicorn instance on your local machine
([deployment documentation](https://netunicorn.github.io/netunicorn/administrator_docs/deployment.html#simplified-deployment))
. After installing the netUnicorn docker instance, create an experiment that
pings the domain ``iitd.ac.in`` for 10 seconds. Report the following:
- List the docker services get initiated when you start the docker instance?
You can read more about these services in Section 4.3 of the netUnicorn
paper.  
- Explain what happens when the two functions, ``prepare_experiment``
and ``start_execution``, are invoked. Hint: Read Figure 3 and Section 4.3 in
the paper. 

## Part 2: Testing Internet Speeds 
Let's use this platform to run a simple
measurement task, i.e., measuring network connection speed. NDT7 and Ookla
Speedtest are two tools that can measure the speed of a network connection. Write an
experiment that runs these two tools one after the other. Report the following: 
- What is your experiment pipeline?
- Before running the experiment, netUnicorn compiler computes the environment
required for succesful execution. Find out the environment definition for
this experiment and explain it. 
- Run the experiment 10 times and compare the
speeds reported by the two tools. Are the reported speeds different between the two tools? Are
there any trends across the 10 runs? Explain the potential reasons for
differences (if any). Hint: [Read](https://cacm.acm.org/research/measuring-internet-speed) how speed test works. 

## Part 3: Measuring Latency under Load 
In this part of the assignment, you will develop a task and add it to the netUnicorn library that will help in mesauring network latency under load or bufferbloat. Latency under load (LuL) is defined as the end-to-end network latency of a network path that is under load. This is an important user exeperience metric that measures the responsiveness of the network in the presence of high traffic. A high LuL value indicates lower responsiveness (e.g., laggy gaming experience or a slow paytm transaction). Your goal is to develop an experiment that measures the latency under load of a connection using netUnicorn. Let's think about how you can measure LuL in the netUnicorn framework. 

An LuL measurement needs to measure latency while the network is loaded. To measure latency we can use ``Ping`` task and to create load we can use ``Ookla SpeedTest``. Ideally, you can run both these tasks in parallel and log the latency measurements. However, there are few challenges when it comes to details. We need to be able to associate latency measurement to the network state, whether it is idle, loaded in upstream direction, or loaded in downstream direction. However, the duration of the test is variable and unknown. Thus, we can not trivially associate a latency measurement to the network state. One workaround is to capture the network traffic while the test is running and process it after the test to figure out the boundaries of download and upload tests. netUnicorn library already contains tasks to start and stop capture. You need to write a task that processes a given capture file to identify the test boundaries, associate the ping measurements to the respective network state, and outputs the LuL (downstream load and upstream load).

- Write a Task ``FindLuLFromPcap`` that performs the above mentioned tasks. 
- Create an experiment that measures the LuL for your connection. Write the pipeline and report your results. For comparison, also report the idle latency (i.e., when no speed test is running).

## Deliverables
- You should submit three different python files, ``part1.py``, ``part2.py``, and ``part3.py``, each implenting the respective parts of the assignment. These files should run in a local deployment environment of netUnicorn. 
- Submit your observations in a separte PDF file. 
