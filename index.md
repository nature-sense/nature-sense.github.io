---
title: Naturesense
layout: home
nav_order: 1
---

# Welcome to Naturesense

> Where nature meets technology

![banner](images/banner.png)

## Naturesense

Naturesense is a project to create solutions that exist at the interface between nature and technology. Or to put it another way, the Naturesense project aims to provide technology solutions to observe, measure, control or manage natural things.

Example applications would be:

- Smart AI-based camera traps for use in academic research, conservation etc.
- Smart agriculture, in particular hydroponic systems.

The project aims to make these solutions accessible by:

- Using and creating open source software.
- Using low-cost IoT sensors, cameras, controller boards etc.
- Using 3d printing with open source designs.

The project adopts an architectural approach based around a set of core technologies:

- Software for edge devices is written in Python (to be replaced by Rust).
- Software for mobile or web applications is written using [Flutter](https://flutter.dev/).
- Software for devices and applications is reactive and asynchronous, with a focus on the  [Actor Model](https://en.wikipedia.org/wiki/Actor_model)
- Communications between devices and apps use Websockets with  [Protocol-buffers](https://protobuf.dev/) used for message definition.
- Communications between devices on a LAN will use  the  [Eclipse Sparkplug protocol](https://sparkplug.eclipse.org/specification/)
- Wide area communications will use  [Meshtastic](https://meshtastic.org/)(LoRa) with Protocolbuffers used for message definition.
- Machine learning will be used where appropriate, focussing on the  [Tencent NCNN  framework](https://github.com/Tencent/ncnn) for interence applications.

The preffered hardware for Naturesense projects is:

- Raspberry Pi for high level devices especially those using Machine Learning.
- Raspberry Pi Pico or ESP32 family for low-level devices.



The Naturesense software and 3d-printer files are available  [here](https://github.com/nature-sense).

## AI Camera Trap for Insects

The first Naturesense project is a low-cost **AI-based Insect Camera Trap**, which can be used to count and identify insects for research purposes, in agriculture and in other areas.

The trap is designed to be easy to use. It provides a WiFi interface and is controlled by an iPhone or iPad mobile application.

The trap is designed as the first stage in an insect identification/counting workflow. Using a camera and an ML model, the trap will detect and track individual insects within its field of view. The trap takes  images of each insect it observes, and attaches metadata. These images/metadata can then be used offline in an insect classification workflow.

The trap provides a WebDav interface which allows a PC or Mac to access the trap as a "network drive", allowing all the images/metadata to be transferred off. On the Mac this can be done directly through the Finder.

The main component in the trap is the **AI camera** which is based around a Raspberry Pi Camera 3 and a Raspberry Pi Compute Module 5 (or CM5). The power of this  board allows AI tasks to be run directly on the processor, without requiring hardware accelleration. The camera is very small measuring only 60mm  x 53mm x 30mm.

The second component is waterproof case that can accomodate the camera and a power bank (battery), and allows the camera to be used outside.

The left image above shows the AI camera, whereas the right image shows the camera in the waterproof case.

















