---
abbrev: ocn-model
docname: draft-km-intarea-ocn-latest
title: Operations and Control Networks - Reference Model and Taxonomy
date:  false
category: info
stream: IETF

ipr: trust200902
area: INTAREA
workgroup: Independent Submission
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
-
  ins: K. Makhijani
  name: Kiran Makhijani
  org: Futurewei
  email: kiran.ietf@gmail.com

informative:

  DETNET-ARCH: RFC8655

  TSNTG:
    target: https://1.ieee802.org/tsn
    title: IEEE, "Time-Sensitive Networking (TSN) Task Group"
    date: 2018

--- abstract

This text formulates a specialized network concept to support communication constraints in automated systems. These
specialized networks, called Operations and Control networks (OCN), are significant to many application scenarios
involving the control and monitoring of mechanical and digital devices. The document defines the OCN reference model,
describing the associated components, interfaces, and reference points. The generalization of OCN concepts and
compute-intensive applications and end-machine. The reference model is independent of any specific technology.
standardized mechanisms will facilitate large-scale machine-to-machine communication and help with integration between

--- middle



# Introduction {#intro}


A number of applications require specialized networks to perform operations (on controller devices) that change
or monitor the behavior of equipment and the environment in which they operate. Such domains benefit from software-driven
process automation with the ability to control and detect changes remotely.

Traditionally, equipment operation is associated with factory automation. However, growth in the Internet of Things (IoT)
has broadened the scope of operations and control to support process automation into various market verticals and commercial
applications. For example, it is possible to remotely control door locks at home and monitor visitors from the
surveillance cameras. In addition, process automation software will automatically detect a familiar face and permit
entry. In an automobile’s car area network (CAN), entire engine operations, speed control, temperature collision
detection, and avoidance mechanisms are performed through intelligent software without human-in-the-loop.
Control processes use real-time power consumption data inputs in large-scale power distribution systems to prevent power outages by performing automatic load re-distribution across different sub-stations.


These scenarios have a common requirement of operating on an end-equipment controlled by presumably an intelligent machine. Such systems vary from general purpose networks in many ways. Their requirements for a reliable network, latency, packet loss, and real-time delivery are precise, and traffic patterns are well defined. Their tolerance to failures is extremely low. Those failures can lead to halting the process and unpredictable behavior causing a loss in production, inability to develop the product in compliance with its specification, road collisions, etc. Moreover, computing is a critical part of modern industrial automation or IoT operation. Combining general purpose compute-centric networks with highly-precise operations networks requires devising new standard mechanisms in networks serving process automation scenarios.

An Operation and Control Network (OCN) is the interconnection of field devices (actuators, sensors) and their associated controllers to exchange data to cause and monitor changes to the end-equipment. Each OCN connection is designed or provisioned to fulfill the traffic characteristics with stringent time and reliability constraints such as protecting bounded latency and not allowing packet losses.

OCN, itself is not a new concept. Other industrial network technologies that would be classified as OCN are available, albeit with limited functionalities and at a smaller scale. However,  process automation is growing across diverse applications and use cases, and a broader, more generalized approach is needed. OCN integrates factory equipment or automation infrastructure beyond a single location to other locations and even the cloud; it additionally integrates existing industrial network technologies. OCN aims to formalize the mechanisms for interaction between the OCN components.

The rest of the document represents the OCN taxonomy and a detail description of OCN concepts.



# Terminology


* Operational technology (OT):
Programmable systems or devices that interact with the physical environment (or manage devices that
interact with the physical environment). These systems/devices detect or cause a
direct change through the monitoring and/or control of devices, processes,
and events. Examples include industrial control systems, building management systems,
fire control systems, and physical access control mechanisms. Source: NIST

* Industry Automation:
Mechanisms that enable the machine-to-machine communication by use of technologies
that enable automatic control and operation of industrial devices and processes leading
to minimizing human intervention.

* Control Loop: Control loops are part of process control systems in with desired process
response is provided as an input to the controller, which performs the corresponding action
(using actuators) and reads the output values.  Since no error correction is performed,
these are called open control loops.

* Feedback Control Loop: Feedback control loop is a system in which the output of a control
system is continuously measured and compared to the input reference value.  The controller
uses any deviation from the input value to adjust the output value for the desired response.
Since there is a feedback of error signal to the input, these are called closed control loops.

* Industrial Control Networks:
The industrial control networks are interconnection of equipments used for the operation, control or
monitoring of machines in the industry environment. It involves different level of communications -
between fieldbus devices, digital controllers and software applications

* Human Machine Interface:
An interface between the operator and the machine. The communication interface relays
I/O data back and forth between an operator's terminal anf HMI software to control and monitor equipment.

{::comment}
* Machine  Machine Interface (MMI): This term is newly introduced for the scope of this document to describe an
interface between the operator terminal software and the machine or from an automation engine to the device. This
interface takes HMI out of the loop, by automating control and monitoring functions through software.
{:/comment}


## Acronyms

* HMI: Human Machine Interface
* OCN: Operations and Control Networks


# Operation and Control Networks

Definition:

> An Operation and Control Network (OCN) supports all the capabilities necessary to accomplish a process
or control command execution on actuators for the desired effect prescribed by the controllers
based on continuous inputs from the sensory data and application requests.


An Operation and Control Network (OCN) connects field devices, and their associated controllers for the exchange of data to trigger and monitor changes to achieve desired effect.

The key functional components of OCN are field devices and controllers. These terms are well-known in the industry control systems and are now extended to newer scenarios of autonomous driving systems or home IoT. The field devices are characterized as sensors and actuators and are associated with physical or logical entity that can be observed, monitored, or caused to move or change. In the scope of this document, field devices have broader meaning than Internet of Things (IoT) since all the field devices may or may not be directly connected to the network. For example legacy fieldbus devices that are connected to a PLC through serial-bus and interface over the  network through those PLCs. Sensors are devices that respond to a physical  (such as heat, light, sound, pressure, magnetism, or a particular motion) or digital (software errors or thresholds) change and report measured values.

> Note: the term “OCN field device” will be used to represent actuator and sensors together.

A related well-known term is Operational Technology (OT). Traditional OT related networks are engineered over limited distance in a specific area; In contrast, OCN extends OT connectivity logically which may flexibly beyond factory premises covering cloud and edge scenarios in which components are disaggregated or are not collocated.

> Note: TODO: Service objectives are wider – be specific to what OT need. Clarify traffic profile.

OCN enables different types of messages across these function elements. The message data sent from controller to actuator is smaller than a typical network payload. However, the packet must not be dropped or lost by the network. Therefore, packet delivery must guaranteed by the OCN. Similarly sensory data itself may also be small but is emitted from several sources with a fixed frequency. Several processes involving both actuators and sensors by continuous monitoring of output data from these two types of devices.

The characteristics of these messages is covered in Section {{characteristics}}.
OCNs will serve different applications and the most common attribute among them is safety of operations. Safety implies several things – that the requested operation or a control command was executed as instructed without any adverse impact to the mechanical equipment or the environment.

The traffic originating from change is triggered through commands delivered to actuator and the same device or different sensor Each OCN connection is designed or provisioned to fulfill the traffic-characteristics with stringent quality of services.

> Note: Suggestions on naming anything is OK. I am not happy with any of these names.
> Note 2: Do we really need sensor and actuator interface separate. Can they not be merged?

~~~~~ drawing
            +----------+ +---------+
            | Actuator | | Sensor  |
            | Point    | | Point   |
            +---^------+ +-----^---+
                |              |
                |AP-I          | SP-I
            +---v--------------v-----+
            | Operations & Control   |
            |       Network (OCN)    |
            +-----------^------------+
                        |
                +-------v------+
                | Controller   |
                |   Point      |
                +--------------+
                      ^
                      | NBI
                      V
                ---------------
                | Applications |
                ---------------
~~~~~
{: #ocn-model title="Operations & Control Networks(OCN) - Reference Model"}

An ‘operation’ in OCN is a process of accessing information from the field-sensors, writing command data, and reading
it back from the actuators. Figure {{ocn-model}} above is a reference model for the OCN. An application executes
operation on field-devices over OCN to monitor specific and/or overall state of the system; An operation may involve realizing
feedback control loop between the controller, actuating and sensory devices.

The OCN Reference model is extended to accommodate a variety of access networks. In fact, it serves as convergent network to
integrate communication across different technology specific networks. Consider it as a specialized shared network infrastructure
that interconnects different other OT-enabled access networks. Some of the examples of OT-enabled networks are Ethernet/IP,
Profinet, TSN {{TSNTG}}, Detnet{{DETNET-ARCH}}, 5G radio, private 5G, etc. As is implied that each of these technologies
are by themselves capable of supporting several properties of OCN. However, all of them are representative of a single access
network instance. It is

A generalized OCN model supports (or requires) the following interfaces:

## Controller Point Interface (CP)
A controller point interfaces with the OCN for accessing sensors and actuators. The controller network has visibility
into application-specific performance or KPIs. These are expressed in terms of network resources for example tolerance
to packet loss, latency-limits, jitter variance, bandwidth, and description of safety. Such capabilities maybe exposed
from OCN over COI, to allow applications in controller access networks to have a broader knowledge about the OCN.
Since OCNs maybe shared among different applications, each with different KPI values for the same resource, a controller interface is used to express specific requirements towards OCN.

An important aspect of controller function element is that it integrate with the IT infrastructure and provides a standard interface with upper layer applications for translating higher-level requests into operational actions.

In most cases, the controllers initiate and request OCN connectivity towards the field device networks, i.e.,
it is unlikely that a field-device would initiate connection towards the controller (unless it is restoration of a disrupted connectivity).

Note: 22.804 uses terms as dependable communication and communication function.
Controller OCN Interface is required to support traffic profiles with time constraints for both periodic and asynchronous communications.

## Actuation Point (AP)

An actuation point is used for the communication between the actuator devices and controllers. The OCN enables control of actuating devices remotely from the controller by meeting all the requirements necessary for a successful command execution. The actuator participates in closed control loop over OCN. The network specific requirement will be applied between the controller and actuator over API {{api-desc}}. Since the control commands change the state of the system, only the verified messages will be delivered to the AP.

## Sensor Point (SP)

The sensor point's main function is to emit periodic data from the sensors. It may intermittently provide asynchronous readings upon request from the controller.

 A sensor point is the functional element from where sensory data is emitted back to controller. SP may receive initial requests to emit data with certain periodicity or  may provide realtime data upon request. The communication to sensor is also through a controller since controller is involved in using sensory data to change parameters in actuators.

 The OCN enables delivery of data emitted from sensor devices to the controller networks by meeting all the application demands requested by controller.


# OCN Characteristics {#characteristics}

The characteristics of OCN differentiates it from general purpose networks available today to provide
user connectivity to provide plethora of services, often not involving machine-to-machine type communications.

## OCN Communication Interfaces {#ifs}

OCN interfaces enable communication between its functional elements, specifically, two interfaces are defined:

### Actuator Point Interface {#api-desc}

Interface between CP and AP is referenced as AP-I.  As the name suggest, AP-I carries out communication between actuation points and controllers. Actuators are designed to write and then read-back parameters or values. Thus,the message may be request write and then read-back in reply type. The latency bounds of such messages must be met in either direction independently.
This interface is a bidirectional.

### Sensor Point Interface{#spi-desc}

   Interface between CP and SP is referenced as SP-I. It describes the set of messages permitted between the sensor points and controller points. Sensors that may be programmed to send periodic sensory data may send data at specified intervals. for the other cases, controller may solicit reading values. The interface should be a bi-directional.

~~~~~drawing
                         .-,,-.
          +--+        .-(  O   )-.       +--+
          |  |<-------;--+ C +----;----->|  |
          +--+  AP-I  '-(| N | ).-'SP-I  +--+
           AP            |---|            SP
                         |   |
                         v   v
                       +-----+
                       | CP  |
                       +-----+

            +---------+  +-------+
            | Actuator|  | Sensor|
            |  Point  |  | Point |
            +---^-----+  +--^----+
                |           |
                |AP-I       | SP-I
            +---v-----------v----+
            |     Controller     |
            |       Point        |
            +---------^----------+
                      |
                      | Northbound interface
                ------v--------
                | Applications |
                ---------------

~~~~~
{: #ocn-ifs title="Interfaces in OCN"}

> Note: Direct sensor to actuator communication is not in the scope of OCN for the following reasons:

  1. In this model, actuator and sensors do not have 'decision-making' capabilities. They perform requested
     tasks as instructed. In other words, they are passive actors. The tasks performed by these field-devices themselves
    maybe autonomous but do not change their behavior or react to changing conditions once programmed. Providing a direct
     communication between sensor and actuator could potentially leave controller out-of-sync or the operations in
     unpredictable state.
  1. It is likely that several applications are interested in reading sensors for offline analysis and do
     not intend to changes the state in control processes. For such cases, it helps to have limited
     set of permitted messages and prevent from executing undesirable actions.

### Northbound Application Interface {#ocn-nbi}

An application domain is a group of actuators, sensors and controller. The application domain will inclue access domains and be interconnected using OCN.

## OCN Message Classification {#msgs}

The specifics and types of messages exchanges over the interfaces are described as follows:

### In-time messages

TODO

### On-time messages

TODO


# IANA Considerations

This document requires no actions from IANA.

# Security Considerations

This document introduces no new security issues.

# Acknowledgements

--- back

