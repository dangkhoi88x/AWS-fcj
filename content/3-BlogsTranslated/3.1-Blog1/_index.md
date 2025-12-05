---
title: "Blog 1"
date: 2025-11-13
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Efficient Video Streaming and Edge AI Vision with Realtek, Plumerai, and Amazon Kinesis Video Streams

Edge artificial intelligence (AI) is becoming increasingly popular in smart video devices. For example, smart home cameras and video doorbells have revolutionized home monitoring. From simple recording tools and remote viewing, these devices have evolved into intelligent observers. With AI integration, today's cameras can proactively analyze scenes, alert users to motion events, recognize familiar faces, detect package deliveries, and flexibly adjust recording behavior. Enterprise surveillance cameras are another example. These cameras have superior resolution, enhanced computing capabilities, and can support more complex AI models. These enhanced capabilities deliver sharper detection at greater distances.

As illustrated, customers demand intelligent monitoring systems that can process data locally while maintaining privacy and reducing bandwidth costs. To meet these needs, the AWS Internet of Things (AWS IoT) team has developed a smart camera solution together with AWS partners, combining Amazon Kinesis Video Streams, Realtek's energy-efficient Ameba Pro2 microcontroller, and efficient machine learning models from Plumerai. This article provides guidance for event-triggered video uploads combined with edge-based human detection algorithm processing.

---

## Solution Architecture

The following diagram illustrates the solution architecture used in this article:

- Starting from the camera, the device firmware has integrated Realtek SDK to access camera modules through defined APIs.

- Video segments are passed to Plumerai's machine learning models to perform object detection.

- The sample application adds detection results as bounding box overlays on the original video segments. This sample application continuously uploads video segments to the cloud through Kinesis Video Streams Producer SDK. (Additionally, you can also configure detection results to trigger uploading 20-second video segments.)

- Kinesis Video Streams Producer SDK uses PutMedia API with long-lived HTTPS connections to continuously upload MKV segments in a streaming fashion.

- Media data will be ingested and the service stores all media data durably for later analysis.

- A user interface application performs live or recorded video playback, based on HLS or DASH protocols from Kinesis Video Streams.

- This solution feeds video and audio data into Large Language Models (LLMs) to gather insights from Agentic AI. (We will cover semantic video search in a follow-up article).

---

## Integration Highlights

### Amazon Kinesis Video Streams

Kinesis Video Streams transforms how businesses handle video solutions for IP cameras, robots, and automobiles. Key benefits include:

- **Fully managed architecture**: This helps technical teams focus on innovation rather than infrastructure, ideal for companies with limited resources.

- **Open-source AWS SDKs**: Leading brands particularly value independence from platform constraints.

- **Flexible pay-as-you-use pricing model**: While device development can take months or years, you don't pay until cameras are operational. With typical cloud storage trigger rates below 30% and decreasing annually, costs are significantly lower than fixed licensing fees.

---

### Plumerai

Plumerai specializes in embedded AI solutions, focusing on making deep learning compact and efficient. Plumerai models help deliver inference on small, affordable, and energy-efficient hardware. The company also optimizes AI models for the Realtek Ameba Pro2 platform through:

- **Assembly-level optimization**: Maximizing Arm Cortex-M CPU performance and leveraging DSP instructions to enhance signal processing capabilities.

- **Neural Architecture Search (NAS)**: Selecting optimal AI models for Realtek's NPU and memory architecture to achieve 0.4 TOPS NPU acceleration.

- Plumerai models use Realtek's on-chip hardware accelerators (scaler, format converter) to offload computation.

- **RTOS-compatible AI models**: Seamless integration with the SoC's real-time operating system.

- Application integration with Realtek's media framework.

- **Fast boot design**: Supporting fast boot times, improving battery life, and ensuring high-speed active object detection.

- Edge AI models trained on 30 million labeled images and videos.

These improvements deliver the following real-world performance:

- Provides accuracy without wasting memory.

- Records wide scenes through 180° field-of-view lenses.

- Detects individuals at distances over 20m (65ft).

- Handles crowds by tracking 20 people simultaneously.

- Maintains individual tracking with unique ID system.

- Operates stably in daylight and complete darkness.

---

### Realtek Ameba Pro2

The figure above illustrates the data architecture of Realtek Ameba Pro2. It includes the Integrated Video Encoder (IVE) and Image Signal Processor (ISP) processing raw media data and passing results to the Video Offload Engine (VOE). VOE then manages multiple video channels and concurrent video streams to support motion detection algorithms. The Neural Processing Unit (NPU) performs inference on images or image regions. The Parallel Processing Unit (PPU) handles multitasking such as cropping Regions of Interest (ROIs) from high-resolution images, resizing NPU inference input, and fetching final output from high-resolution channels. This architecture unlocks powerful capabilities to support edge video analytics, including:

- Operates with minimal CPU power for maximum efficiency.

- Near real-time response to motion.

- Starts video processing even during boot.

- Transmits to both SD card and cloud through secure WiFi or Ethernet.

- Leverages NPU to deliver superior AI performance.

- Integrates with Plumerai models and Kinesis Video Streams through media framework SDK.

---

## Step-by-Step Guide

This section outlines the steps to build the solution for running edge AI and streaming video segments.

### Prerequisites

- AWS account with permissions for:
  - Logging into AWS Console.
  - Kinesis Video Streams APIs (GetDataEndpoint, DescribeStream, PutMedia).
  - Creating Amazon Elastic Compute Cloud (Amazon EC2) instances to build SDK and binary files.

- A stream resource named "kvs-plumerai-realtek-stream" created on Kinesis Video Streams Console.

- Realtek Ameba Pro2 Mini MCU.

- Basic knowledge of embedded systems and working in Linux environments.

- Internet connection to download SDK and upload videos to AWS.

- Library files and machine learning models from Plumerai. (Please submit your request on the Plumerai website.)

---

## Build Environment Setup

This article uses an Amazon EC2 with Ubuntu LTS 22.04 as the build environment. You can also use your own Ubuntu computer to cross-compile the SDK.

### Setting up Amazon EC2 Instance:

1. Log into AWS Management Console and navigate to Amazon EC2.

2. Launch an instance with the following configuration:
   - Instance name: KVS_AmebaPlumerAI_poc
   - Application and OS Images: Ubuntu Server 22.04 LTS (HVM)
   - Instance type: t3.large
   - Create a new key pair for login: kvs-plumerai-realtek-keypair
   - Storage configuration: 100GiB
   - Configure SSH connection prerequisites to allow SSH traffic.

3. Download sample scripts from Github:

Use the following command to log into your Amazon EC2 instance (make sure to replace xxx.yyy.zzz with your instance's IP address). For detailed instructions, see Connect to your Linux instance using an SSH client.

```bash
ssh -o ServerAliveInterval=60 -i kvs-plumerai-realtek-keypair.pem ubuntu@54.xxx.yyy.zzz
```

```bash
git clone https://github.com/aws-samples/sample-kvs-edge_ai-video-streaming-solution.git
cd ./sample-kvs-edge_ai-video-streaming-solution/KVS_Ameba_Plumerai
```

### Getting Plumerai Library:

Use Plumerai's contact form to request a copy of their demo package. Once you have the package, replace the "plumerai" directory with it in the Amazon EC2 instance. The updated directory structure should be as follows:

```
KVS_Ameba_Plumerai/
├── plumerai/
│   ├── include/
│   ├── lib/
│   └── models/
└── ...
```

### Getting Ameba SDK:

Please contact Realtek to obtain the latest Ameba Pro2 SDK. In the directory structure, replace "ambpro2_sdk" in Amazon EC2. The directory structure should be as follows:

```
KVS_Ameba_Plumerai/
├── ambpro2_sdk/
│   ├── component/
│   ├── project/
│   └── ...
└── ...
```

### Installing Dependencies and Configuring Environment

Run the `setup_kvs_ameba_plumerai.sh` script in the `sample-kvs-edge_ai-video-streaming-solution` directory from the Github repository:

```bash
chmod +x setup_kvs_ameba_plumerai.sh
./setup_kvs_ameba_plumerai.sh
```

The script will automatically install Linux dependencies, build the Realtek toolchain, run necessary Plumerai patches, copy model files, and download Kinesis Video Streams Producer SDK. If you encounter errors during this process, please contact Realtek or Plumerai for technical support.

---

### Sample Configuration in Kinesis Video Streams Producer SDK

Use the following information to configure AWS credentials, stream name, and AWS region. This information can be found in the `component/example/kvs_producer_mmf/sample_config.h` file.

```c
// KVS general configuration
#define AWS_ACCESS_KEY "xxxxx"
#define AWS_SECRET_KEY "xxxxx"

// KVS stream configuration
#define KVS_STREAM_NAME "kvs-plumerai-realtek-stream"
#define AWS_KVS_REGION "us-east-1"
#define AWS_KVS_SERVICE "kinesisvideo"
#define AWS_KVS_HOST AWS_KVS_SERVICE "." AWS_KVS_REGION ".amazonaws.com"
```

Comment out `example_kvs_producer_mmf();` and `example_kvs_producer_with_object_detection();` in the file `/home/ubuntu/KVS_Ameba_Plumerai/ambpro2_sdk/component/example/kvs_producer_mmf/app_example`.

```c
//example_kvs_producer_mmf();
//example_kvs_producer_with_object_detection();
```

---

## Compilation and Building

Use the following code to navigate to the KVS_Ameba_Plumerai directory and compile and build the updates.

```bash
cd ./KVS_Ameba_Plumerai
cmake -DVIDEO_EXAMPLE=ON -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE=../ambpro2_sdk/project/realtek_amebapro2_v0_example/GCC-RELEASE/toolchain.cmake -Sambpro2_sdk/project/realtek_amebapro2_v0_example/GCC-RELEASE -Bbuild
cmake --build build --target flash_nn
```

---

## Running the Sample on Ameba Pro2

### Downloading and Flashing Binary Image:

1. Download the binary image `flash_ntz.nn.bin` to your local machine from the Amazon EC2 instance. For example, run the following command on your local machine (make sure to update the command to include your IP address and correct directory path):

```bash
scp -i kvs-keypair.pem ubuntu@54.64.xxx.xxx:/home/ubuntu/sample-kvs-edge_ai-video-streaming-solution/KVS_Ameba_Plumerai/build/flash_ntz.nn.bin ./flash_ntz.nn.bin
```

2. Connect the Ameba Pro2 MCU to your local machine via USB and press the buttons on both sides to enter download mode. Use the Ameba Pro2 image tool from Realtek to flash the binary image and restart it.

For example, use the following command on Microsoft Windows (please update your own path to the tool and COM port number):

```bash
C:\Users\Administrator\Desktop\Pro2_PG_tool_v1.3.0>.\uartfwburn.exe -p COM3 -f flash_ntz.nn.bin -b 2000000 -U
```

Or use the following command on macOS:

```bash
./uartfwburn.arm.darwin -p /dev/cu.usbserial-110 -f ./flash_ntz.nn.bin -b 3000000
```

When the process completes, you will receive output logs similar to the following:

```
[INFO] Flash firmware successfully!
```

---

### WiFi Configuration:

1. Press the reset button, located next to the red LED indicator.

2. Use a serial tool and configure it as follows:
   - Baud rate = 115200
   - Data bits = 8
   - Parity = None
   - Stop bits = 1, XON_OFF

3. Paste WiFi configuration information (make sure to change the information specific to your network):

```
ATW0=myHotspotName
ATW1=myPassword
ATWC
```

When you're done, press Enter.

When the process completes, you will receive output logs similar to the following:

```
[INFO] WiFi connected successfully!
```

---

## Verifying Video on AWS Management Console

Keep the Ameba Pro2 connected to the USB port and point the camera to record human movements.

Open the Kinesis Video Streams Console and select the video stream you created. You will see video with bounding boxes displaying detection results.

Video segments with bounding boxes for people and their faces have now been successfully ingested and durably stored by the service.

---

## Performance Summary

According to laboratory test results, the edge application requires only 1.5MB ROM capacity and is optimized for Ameba Pro2's NPU. The firmware achieves processing speeds of approximately 10 frames per second while consuming only 20% CPU. This leaves capacity for additional applications.

---

## Cost and Cleanup

Typically, you will complete all compilation and testing steps within 10 hours. Total cost should be under $5 USD. For details on Amazon EC2 pricing, see [Amazon EC2 On-Demand Instance Pricing](https://aws.amazon.com/ec2/pricing/on-demand/). For details on Kinesis Video Streams pricing, see [Kinesis Video Streams Pricing](https://aws.amazon.com/kinesis/video-streams/pricing/). Our sample application includes the following three parts:

- Data ingested into Kinesis Video Streams
- Data consumed from Kinesis Video Streams using HLS
- Data stored in Kinesis Video Streams

To save costs, please delete the resources you created:

- Open Amazon EC2 Console and terminate the `KVS_AmebaPlumerAI_poc` instance.
- Open Kinesis Video Streams Console and delete the `kvs-plumerai-realtek-stream` stream.

---

## Conclusion

For additional guidance on video applications, see:

- [Consuming Media Data from Kinesis Video Streams](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/consuming-streams.html)
- [Amazon Kinesis Video Streams Examples](https://github.com/aws-samples/amazon-kinesis-video-streams-examples)
- [amazon-kinesis-video-streams-media-viewer](https://github.com/aws-samples/amazon-kinesis-video-streams-media-viewer) for quick experimentation and testing.
- Watch the video [Connect Your Camera with Amazon Kinesis Video Streams for WebRTC in 10 Minutes](https://www.youtube.com/watch?v=example) on the AWS IoT YouTube channel.

The collaboration between Amazon Kinesis Video Streams, Realtek, and Plumerai delivers an advanced home security solution, leveraging edge AI vision and enhanced video streaming capabilities. This integrated system addresses the growing demand for complex AI/ML video solutions in both smart home and enterprise surveillance markets. This collaborative solution also illustrates the potential for AI-based improvements in home and enterprise security by providing enhanced monitoring capabilities, efficient processing, and seamless cloud integration.

With AI detection capabilities directly on the device, this edge-first approach means your video data stays local until needed, protecting privacy while significantly reducing bandwidth usage. As demand for smart video solutions continues to grow, this technology sets a new standard for what can be achieved in intelligent monitoring and video surveillance systems.
