
# Federated CPU Tracker with SyftBox - Day 8 Reflection

Continuing my **30 Days of Open Learning** journey, **Day 8** was dedicated to deepening my understanding of the **Federated CPU Tracker** project using **SyftBox**. 

Building upon the API foundations from **Day 7**, today I focused on exploring the **CPU Tracker Leader** component. This exploration was crucial for comprehending how CPU usage data from multiple network members can be aggregated and visualized in a privacy-preserving manner. 

Here’s a comprehensive look at what I learned and the experiences I had during this phase of my learning.

---

## 🛠️ Learning Advancement

### From Member to Leader 🔄
After studying the **CPU Tracker Member** on Day 7, today's objective was to understand the **CPU Tracker Leader**. This leader component is responsible for:
- **Identifying active network participants** who are sharing their CPU data.
- **Aggregating the collected data** while ensuring privacy is maintained.
- **Visualizing the aggregated data** through an interactive dashboard.

Exploring the transition from a single member to a leader within a federated network introduced me to the complexities associated with data aggregation and real-time visualization, enhancing my understanding of federated systems.

---

## 📖 Key Insights

### 1️⃣ **Environment Setup**
I began by examining the environment setup required for the leader component. Learning about the `run.sh` script highlighted the importance of automating the initialization process, ensuring that the leader component could seamlessly integrate with SyftBox. Organizing the project structure effectively was essential for maintaining clarity and facilitating future exploration.

```bash
# run.sh: Example of initialization
#!/bin/bash
source ~/syftbox/env/bin/activate
python main.py
```

**Insight:** A well-structured project environment and automated setup streamline the learning process and minimize potential integration issues.

### 2️⃣ **Network Participant Discovery**
One of the core functionalities I explored was the ability to dynamically detect active network members. By scanning designated directories where peers publish their sanitized CPU data, the leader component can identify and interact with active participants.

```python
def network_participants(datasite_path: Path):
    """Retrieves a list of user directories (participants) in a given datasite path."""
    entries = os.listdir(datasite_path)
    return [entry for entry in entries if Path(datasite_path / entry).is_dir()]
```

💡 **Insight:** Implementing a dynamic discovery mechanism is vital for scalability, allowing the system to adapt seamlessly as members join or leave the network.

### 3️⃣ **Privacy-Preserving Aggregation**
Understanding how to preserve the privacy of individual participants during data aggregation was a major focus. While Differential Privacy techniques are slated for future enhancements, the current implementation emphasizes data freshness and integrity. By validating that data is recent and sanitized before aggregation, the system maintains both accuracy and privacy.

```python
def get_network_cpu_mean(datasites_path: Path, peers: list[str]) -> Tuple[float, list[str]]:
    """Calculates the mean CPU usage across network peers."""
    aggregated_usage = 0
    active_peers = []

    for peer in peers:
        tracker_file = datasites_path / peer / "api_data/cpu_tracker/cpu_tracker.json"
        if tracker_file.exists():
            with open(tracker_file, "r") as f:
                data = json.load(f)
                if is_updated(data["timestamp"]):
                    aggregated_usage += float(data["cpu"])
                    active_peers.append(peer)
                    
    mean_usage = aggregated_usage / len(active_peers) if active_peers else -1
    return mean_usage, active_peers
```

💡 **Insight:** Validating data freshness and integrity is essential for accurate and reliable aggregation, ensuring the system remains both private and robust.

### 4️⃣ **Historical Data Management**
Managing a rolling history of CPU usage data was another important area of exploration. Implementing a rolling window of CPU usage samples allows the dashboard to display trends over time without becoming bogged down by excessive data. This approach ensures that the system remains both informative and performant.

```python
def truncate_file(file_path: Path, max_items: int, new_sample: float, peers: list[str]):
    """Manages historical CPU data, maintaining a rolling window of samples."""
    history = [{"cpu": new_sample, "timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S")}]
    if file_path.exists():
        with open(file_path, "r") as f:
            history.extend(json.load(f)["items"][-max_items:])
    with open(file_path, "w") as f:
        json.dump({"items": history, "peers": peers}, f, indent=4)
```

💡 **Insight:** Efficient historical data management enhances the user experience by providing meaningful insights without compromising system performance.

### 5️⃣ **Interactive Web Dashboard** 🎨
Exploring the creation of the web dashboard was particularly rewarding. The dashboard offers real-time updates on CPU usage, visualizes historical trends, and displays a list of active peers. This real-time feedback is invaluable for monitoring system performance and understanding network-wide resource utilization.

Here's how the dashboard refreshes every 10 seconds:

```javascript
async function loadCPUTracker() {
    const response = await fetch("./cpu_tracker.json");
    const data = await response.json();
    updateCPUTracker(data);
    document.getElementById("lastUpdate").textContent = new Date().toLocaleString();
}
document.addEventListener("DOMContentLoaded", () => {
    loadCPUTracker();
    setInterval(loadCPUTracker, 10000);
});
```

💡 **Insight:** Real-time visualization tools are crucial for providing immediate insights and facilitating informed decision-making within federated systems.

---

## 🌐 Privacy and Security

Throughout the exploration process, maintaining robust privacy and security measures was paramount. Key strategies included:
- **Aggregated Data Sharing:** Only sharing aggregated metrics to ensure individual data points remain confidential.
- **Opt-In Participation:** Allowing peers to join or leave the network voluntarily by managing their participation through the member application.
- **Data Freshness Validation:** Ensuring that only recent and relevant data is used in the aggregation process to maintain accuracy and reliability.

**Insight:** Implementing strong privacy and security protocols builds trust among network participants and ensures the integrity of the federated system.

---


## 🤔 Reflections

Today's exploration of the **CPU Tracker Leader** was both challenging and fulfilling. Understanding how to aggregate data while ensuring privacy and providing real-time insights highlighted the complexities of federated systems. Key takeaways include:
- **Holistic Understanding:** Combining backend data processing with frontend visualization offers a comprehensive perspective on building effective monitoring systems.
- **Privacy is Paramount:** Implementing privacy-preserving techniques is essential in federated learning to protect individual data while still extracting meaningful insights.
- **Scalability Matters:** Designing systems that can dynamically adapt to changing network conditions ensures long-term viability and effectiveness.

This experience has deepened my understanding of federated learning and the importance of privacy-enhancing technologies in modern data-driven applications.

---

## 🔮 Looking Ahead

With a solid understanding of the leader component, the next steps involve:
- To checkout different application API to see understand the pattern 
- Potentially integrating everything we learned in our RING APP project.

---

💻 ** [Original API Source Code and explanation](https://syftbox-documentation.openmined.org/cpu-tracker-2) **

