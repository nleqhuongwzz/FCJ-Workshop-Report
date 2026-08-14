---
title: "Blog 1"
date: 2026-08-14
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Building a Smart Agriculture System with Multi-Agent Architecture on AWS IoT Greengrass

If there is one field where deploying technology faces the most physical barriers, it is Smart Agriculture. Farms are often located in remote areas with unstable internet, while data from sensors (soil moisture, temperature) or cameras (analyzing crop health) requires real-time processing to make immediate irrigation decisions. Pushing all raw data to the Cloud before processing is an expensive approach with high latency.

Recently, I spent time exploring a very interesting solution from the AWS ecosystem: combining an AI Agent architecture with **AWS IoT Greengrass** to bring AI reasoning capabilities directly down to edge devices — specifically a Raspberry Pi 5 board.

## WHY THIS PROBLEM NEEDS AI AT THE EDGE DEVICE (EDGE AI)?

If we only use ordinary microcontroller (MCU) boards to read moisture sensors and write if-else logic (for example: `if soil_moisture < 30% then turn_on_pump()`), the system becomes rigid and cannot analyze crop disease images. However, if we apply an Agentic Workflow model right at the edge device, the system can divide the work among specialized agents:

- **Camera/Vision Agent**: Automatically captures images of crops, analyzes leaf health and pests using Amazon Bedrock (vision models).
- **Sensor Agent**: Continuously reads real-time data from soil moisture and temperature sensors.
- **Orchestrator Agent**: Aggregates data from both Camera and Sensor, then automatically decides whether to irrigate and how much water is enough.

## HIGHLIGHTS FROM AWS IOT GREENGRASS & STRANDS AGENTS

When I dug deeper into how to implement this architecture on AWS IoT Greengrass, I found three components that smoothly solve the technical barriers:

- **Local/Offline AI Processing**: Thanks to AWS IoT Greengrass, the AI Agent components (specifically Strands Agents) can run directly on the Raspberry Pi. Even if the farm loses network connection, the AI can still read sensors and make automatic irrigation decisions.
- **Device Lifecycle Management**: Deploying code, updating AI models, or changing logic for thousands of Raspberry Pi devices across different farms is done automatically via Greengrass (Over-The-Air updates) without engineers having to physically visit the site to plug cables.
- **Flexible Multi-Platform Integration**: The agent can both communicate with hardware (GPIO pins to turn the water pump on/off) and build a Local Web Dashboard so farmers can monitor directly at the farm.

## PERSPECTIVES GAINED FROM EXPLORING THIS TOPIC

What I like most about this architecture is that it redefines how we do IoT: from passive data-collection devices (Dumb Sensors) to devices that can reason and act independently (Autonomous Edge AI).

The system can now not only "report" that soil moisture is 20%, but also "see" a wilting plant, assess the overall situation, and decide to pump water immediately. This is truly a major transformation in applying AI to real production.

## REFERENCES

- AWS Internet of Things Blog. *Build smart agriculture with AWS IoT Greengrass and Strands Agents.*
- Amazon Web Services. *AWS IoT Greengrass Developer Guide.*

Ho Chi Minh City, August 2026
Huynh Phuc Hung

[Blog link at AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2240841880014105/)
