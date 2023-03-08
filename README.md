
<p>

[![License: MIT](https://img.shields.io/badge/License-MIT-gree.svg)](https://opensource.org/licenses/MIT?style=plastic) 
<a href="#!" target="_blank"><img src="https://img.shields.io/static/v1?label=build&message=passing&color=gree?style=flat"/></a>


<a href="http://users.softnet.tuc.gr/~minos/Papers/edbt19.pdf" target="_blank"><img src="https://img.shields.io/static/v1?label=&message=Functional Geometric Monitoring&color=gree?style=plastic"/></a>
<a href="http://dimacs.rutgers.edu/~graham/pubs/papers/cmencyc.pdf" target="_blank"><img src="https://img.shields.io/static/v1?label=&message=Count-Min&color=gree?style=plastic"/></a>
<a href="http://dimacs.rutgers.edu/~graham/pubs/papers/streamsnet.pdf" target="_blank"><img src="https://img.shields.io/static/v1?label=&message=Fast-AGMS&color=gree?style=plastic"/></a>

<a href="#!"><img src="https://img.shields.io/static/v1?label=&message=Continuous Monitoring&color=blue?style=plastic"/></a>
<a href="#!"><img src="https://img.shields.io/static/v1?label=&message=Distributed stream processing&color=blue?style=plastic"/></a>
<a href="#!"><img src="https://img.shields.io/static/v1?label=&message=Scalability&color=gree?style=plastic"/></a>

<a href="https://flink.apache.org/" target="_blank"><img src="https://img.shields.io/static/v1?label=&message=Apache Flink &color=blue?style=plastic"/></a>
<a href="https://kafka.apache.org/" target="_blank"><img src="https://img.shields.io/static/v1?label=&message=Apache Kafka&color=gree?style=plastic"/></a>

</p>

## Description

> Integration of the Functional Geometric Monitoring (FGM) method in the Apache Flink platform


Functional Geometric Monitoring is a technique that can be applied to any monitoring problem in order to perform distributed and scalable monitoring with minimal communication cost.The FGM method is a method that is independent of the monitoring problem, to achieve this the method uses a problem-specific family of functions termed safe functions.Finally, the FGM method can be naturally adapted under adverse conditions of the monitoring problem such as very tight monitoring bounds and the presence of skew in the distribution of data among the distributed nodes.

## Project Structure

The project structure was organized as follow:

* Worker
  * Worker logic
  * Worker structure
* Coordinator
  * Coordinator logic
  * Coordinator structure
* State
  * Coordinator state
  * Worker State
* Datatypes
  * Vector
  * Sketches
    * Count-Min
    * AGMS 
* SafeZone
* Job
  * JobIteration
  * JobKafka


## Architecture

The two main components of the architecture are the Workers and the Coordinator. Each of these operators is a [KeyedCoProcess](https://nightlies.apache.org/flink/flink-docs-master/docs/dev/datastream/operators/process_function/#the-keyedprocessfunction) operator. In particular, the Workers operator has two inputs, the first refers to the **Input** source and the second to the **Feedback** source that contains the control messages from the Coordinator. Respectively the Coordinator has two inputs, the first refers to the control messages from the Coordinator, and the second to the user-posed queries(**Query** source). Each of these operators has a [side-output](https://nightlies.apache.org/flink/flink-docs-master/docs/dev/datastream/side_output/). The side-outputs in this case act as a **logging** mechanism that records metrics/information about the system such as the communication cost, throughput, latency, and back-pressure.
Regarding the implementation of the  feedback loop, there are two approaches.The first implementation uses as feedback loop a **Kafka topic** that acts as buffer between the Workers and the Coordinator.The second implementation uses as feedback loop the build-in operator of [Iterative stream](https://nightlies.apache.org/flink/flink-docs-master/docs/dev/dataset/iterations/).


<p align="center">

![Alt text](img/readme/abstract_project_architecture.png)

</p>


## Configuration
Depending on the monitoring problem all you need to do is configure the Vector and SafeZone class.
