# MLOps Orchestration: Streaming Threat Ingestion Engine

## Overview
This repository serves as the deployment architecture for the `probabilistic-threat-classifier`. While training an AI model proves analytical capability, deploying it into a live SOC requires an entirely different engineering discipline. This microservice demonstrates how to transition a static, notebook-bound neural network into a high-velocity, asynchronous streaming pipeline capable of processing thousands of forensic events per second.

## Core MLOps Capabilities

* **Asynchronous Message Brokering:** Simulates an enterprise Pub/Sub architecture (Apache Kafka, AWS Kinesis), continuously ingesting JSON telemetry streams without blocking the main execution thread.
* **Dynamic Batching for GPU Optimization:** Deep learning models (like Transformers) process data highly inefficiently when fed logs one-by-one. This engine implements a dynamic queueing system that accumulates incoming events into mathematical tensors (batches) prior to inference. 
* **Strict Latency Windows:** The batching queue is governed by a strict Maximum Latency constraint (100ms). If a batch is not filled within the operational window, the engine forces the execution, ensuring no single security event is held hostage in the queue during low-traffic periods.
* **Automated Triage Routing:** Extracted threat probabilities are immediately evaluated against the Tiered Severity Matrix, simulating real-time WebHook alerts for "Tier 5: BLOCK" categorizations to drive automated SOAR remediation.

## Execution & Usage
This service relies heavily on Python's native `asyncio` library to achieve concurrency.

```bash
# Execute the asynchronous streaming simulation
python async_mlops_inference_node.py
