# Assignment 2 - Active Measurements using NetUnicorn Platform

In class, we discussed NetUnicorn, a platform for conducting reproducible active measurements. The goal of this assignment is to gain practical experience with it. NetUnicorn supports running measurements on different kinds of infrastructure, such as Amazon AWS instances, Raspberry Pi, and Docker containers. In this assignment, you will work with a local environment using a Docker container.

## Part 1: Installing netUnicorn

Your first task is to deploy a Docker NetUnicorn instance on your local machine ([deployment documentation](https://netunicorn.github.io/netunicorn/administrator_docs/deployment.html#simplified-deployment)). After installing the NetUnicorn Docker instance, create an experiment that pings the domain  `iitd.ac.in` for 10 seconds. Answer the following:

- Which services get initiated when you start the docker instance?
You can read more about these services in Section 4.3 of the netUnicorn paper.

- Explain what happens when the two functions, `prepare_experiment`
and `start_execution`, are invoked. Hint: Read Figure 3 and Section 4.3 in
the paper. 

## Part 2: Testing Internet Speeds 
Let's use this platform to run a simple measurement task, i.e., measuring network connection speed. NDT7 and Ookla Speedtest are two tools that can measure the speed of a network connection. Write an experiment that runs these two tools one after the other. Answer the following:

- What is your experiment pipeline?
- Before running the experiment, the NetUnicorn compiler computes the environment required for successful execution. Find out the environment definition for this experiment and explain briefly.
- Run the experiment 10 times and compare the
speeds reported by the two tools. Do you observe consistent differences in the reported speeds between the two tools? Explain the potential reasons for
differences (if any). Hint: [Read](https://cacm.acm.org/research/measuring-internet-speed) how speed test works. 

## Part 3: Measuring Latency under Load 
In this part of the assignment, you will develop a task to add to the NetUnicorn library, aimed at measuring network latency under load or bufferbloat. Latency under load (LuL) is defined as the end-to-end network latency of a network path that is under load. This metric is crucial for user experience as it measures the responsiveness of the network in high-traffic conditions. A high LuL value indicates lower responsiveness, such as a laggy gaming experience or slow transaction on platforms like Paytm. Your objective is to design an experiment within the NetUnicorn framework that accurately measures the latency under load of a connection. Let's consider how you can measure LuL within the netUnicorn framework. 

An LuL measurement needs to measure latency while the network is loaded. To measure latency we can use ``Ping`` task and to create load we can use ``Ookla SpeedTest``. Ideally, you can run both these tasks in parallel, log the latency measurements, and be done with it. However, several challenges arise when considering the details.

A speed test consists of sequentially measuring downlink and uplink path for an unknown and variable duration. To correctly measure bufferbloat,  
we need to be able to associate latency measurements to the underlying network state, i.e., whether it is idle, loaded in upstream direction, or loaded in downstream direction. Given the variable test duration, we can not trivially associate a latency measurement to the network state. 

One workaround is to capture network traffic while the test is running and process it afterward to determine the boundaries of download and upload tests. The NetUnicorn library already includes tasks for starting and stopping captures. Your task is to write a task that processes a given capture file to identify the test boundaries, associate the ping measurements with the respective network state, and output the LuL (downstream load and upstream load)

One way to capture the network traffic while the test is running and process it after the test to figure out the boundaries of download and upload tests. netUnicorn library already contains tasks to start and stop capture. Thus, the only task that needs to be added (by you) is a task that processes a given capture file to identify the test boundaries, associate ping measurements with the respective network state, and calculate the LuL for both downlink and uplink directions.

Thus, you need to:

- Write a Task ``FindLuLFromPcap`` that performs the above-mentioned tasks. 
- Create an experiment to measure the LuL of your connection. Write the pipeline and report your results. For comparison, also report the idle latency (i.e., when no speed test is running).

## Deliverables
- You should submit three different Python files: `part1.py`, `part2.py`, and `part3.py`, each implementing the respective parts of the assignment. These files should be designed to run in a standalone manner with a local netUnicorn environemnt. 
- Submit your observations in a single PDF file.


