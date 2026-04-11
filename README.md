<div align="center">
 
# Big Data Applications
### Real-Time Insurance Fraud Detection Pipeline
 
[![Python](https://img.shields.io/badge/Python-3.x-3776ab?style=flat-square&logo=python&logoColor=white)]()
[![Kafka](https://img.shields.io/badge/Apache%20Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white)]()
[![Flink](https://img.shields.io/badge/Apache%20Flink-E6526F?style=flat-square&logo=apacheflink&logoColor=white)]()
[![Institution](https://img.shields.io/badge/UPMH-Big%20Data-6a0dad?style=flat-square)]()
 
</div>
 
## Overview
End-to-end real-time fraud detection system for insurance claims processing. Combines streaming data ingestion, in-stream ML inference, and low-latency storage for sub-second decision making.
 
## Architecture
 
```
Claims Stream → Apache Kafka → PyFlink (XGBoost inference)
                                      ↓
                             Redis (feature cache)
                                      ↓
                            PostgreSQL (audit log)
```
 
## Key Components
| Component | Role |
|:---|:---|
| **Apache Kafka** | Event streaming & ingestion |
| **PyFlink** | Stream processing & feature computation |
| **XGBoost** | Real-time fraud scoring |
| **Redis** | Sub-millisecond feature caching |
| **PostgreSQL** | Persistent audit and results storage |
 
---
*Maestría en Inteligencia Artificial · Big Data · Universidad Politécnica Metropolitana de Hidalgo*
