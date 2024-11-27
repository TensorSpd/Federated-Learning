# Day 6: [Understanding the SyftBox Computation Model](https://syftbox-documentation.openmined.org/computation-model)

Welcome to Day 6 of the **30DaysOfFLCode** series! Today, I learned the high level view of **SyftBox Computation Model**,  its main components and workflow.

## Introduction

- **APIs**: Facilitate proposing and participating in computations within SyftBox.
- **Datasites**: Represent individual entities (like individuals or organizations) in the Syft network, holding both private and public data.


## Workflow Overview

SyftBox enables computations on private data through a collaborative workflow involving two main roles:

1. **Data Owners**
2. **API Developers**

### Roles Defined

- **Data Owners**: These are entities that possess private data and participate in computations by installing specific APIs. They maintain control over their data and the code that runs on their Datasites.
  
- **API Developers**: These participants propose computational studies. They create and deploy APIs that process data on both the Data Owners' Datasites and their own to aggregate results.

### Workflow Steps

1. **Data Preparation**
   - Data Owners (e.g., Datasites A and B) prepare their private data in supported formats like CSV or JSON.

2. **API Installation by Data Owners**
   - Data Owners install the `data_owner_api` on their Datasites based on the developer's proposal.

3. **API Installation by Developer**
   - The API Developer (e.g., Datasite X) installs the `aggregator_api` on their own Datasite.

4. **Aggregation and Results**
   - The developer's Datasite aggregates the public results from Data Owners, producing the final aggregated result.

**Note**: We need to ensure all participants have their SyftBox clients running and connected to the same Syft network to enable seamless synchronization of intermediary results.

## Example: CPU Tracker API

To illustrate the SyftBox Computation Model, let's consider the **CPU Tracker API**. This application collects CPU usage data from various Datasites and aggregates it to display an average CPU load across the network.

### Live Demonstration
[View a live example of the CPU Tracker API on the Syft network.](https://syftbox.openmined.org/datasites/aggregator@openmined.org/)


## SyftBox Directory Structure

Understanding the directory layout is essential for managing APIs and Datasites effectively.
For example, let's take cpu_tracker for instance, our directory structure would look like this.

- **SyftBox**
  - **datasites**
    - *(other datasites)*
  - **apis**
    - *(other APIs)*
    - **cpu_tracker_member**
      - `main.py`
      - `run.sh`
      - *(additional files)*

- **SyftBox**
  - **apis**
    - **cpu_tracker_member**
  - **datasites**
    - **YOUR_DATASITE**
      - **api_data**
        - **cpu_tracker**
          - `cpu_tracker.json`


## How the CPU Tracker API Works

The **CPU Tracker API** consists of two main files:

- **main.py**
  - Collects 50 CPU usage data points at set intervals.
  - Averages the data while adding noise to protect privacy.
  - Saves the processed result in a public folder for aggregation.

- **run.sh**
  - Automates the execution of `main.py`.
  - Ensures continuous operation and data processing.

The API generates a `cpu_tracker.json` file within one's Datasite's `api_data` folder, containing one's average CPU usage data for a specified period.

## Next Up
- I will in more depth and see how one can build API from scratch for syftbox.


