# Production Line Optimization Report

## Executive Summary

This report presents an analysis and optimization of a factory production line modeled using discrete-event simulation. The baseline system exhibited very long lead times and low machine utilization despite sufficient production capacity. Analysis revealed that the primary constraint was not machine capacity, but downstream logistics and batching behavior, which caused blocking and starvation effects throughout the system. An optimized configuration focusing on buffer sizing, finished goods storage, and shipping frequency significantly reduced lead time while improving flow stability. The results demonstrate important trade-offs between throughput and responsiveness, and resiliency considerations are discussed before presenting a final recommendation.

---

## 1. Baseline Analysis

### 1.1 KPI Measurements

The baseline simulation was run for 168 hours using the provided configuration.

| KPI | Baseline Value |
|----|---------------|
| Throughput (orders/hour) | 4.44 |
| Average Lead Time (hours) | 28.80 |
| Maximum Lead Time (hours) | 56.16 |
| Machine A Utilization | 37% |
| Machine B Utilization | 52% |
| Machine C Utilization | 30% |
| Average Warehouse Inventory | 389 parts |
| Avg Buffer A → B Level | 4.59 / 5 |
| Avg Buffer B → C Level | 0.42 / 5 |
| Average Products per Shipment | 20 |

---

### 1.2 Bottleneck Identification

Although Machine B has the longest processing time, utilization data shows that no machine operates near full capacity. Machine B utilization is approximately 52%, while Machines A and C are significantly underutilized. This indicates that the system does not suffer from a classical machine bottleneck.

Instead, the primary constraint is downstream logistics and flow control. Finished goods are shipped only when a lorry is fully loaded, which introduces batching behavior. Combined with limited finished goods storage and small buffers, this causes Machine C to become blocked, propagating upstream starvation and blocking. As a result, machines remain idle while orders experience excessive waiting time, leading to very high lead times.

---

## 2. Optimization Strategy

The optimization strategy focuses on improving flow continuity and reducing waiting time rather than increasing raw machine capacity.

---

### 2.1 Change 1: Increase Buffer Sizes

**Parameters changed:**
- Buffer A → B: 5 → 12  
- Buffer B → C: 5 → 8  

**Rationale:**  
Larger buffers decouple machines and absorb variability caused by stochastic machine failures. This reduces blocking at Machine A and starvation at Machine C.

**Expected Impact:**  
Improved flow stability and reduced waiting caused by frequent blocking and starvation.

**Trade-off:**  
Increased work-in-progress (WIP) inventory and potentially higher holding costs.

---

### 2.2 Change 2: Increase Finished Goods Storage

**Parameter changed:**
- Finished goods storage capacity: 30 → 80  

**Rationale:**  
Increasing storage capacity prevents Machine C from blocking when shipments are delayed or waiting to fill a lorry.

**Expected Impact:**  
Reduced downstream blocking and smoother system flow.

**Trade-off:**  
Higher finished goods inventory and space requirements.

---

### 2.3 Change 3: Adjust Shipping Batch Size

**Parameter changed:**
- Lorry capacity: 20 → 10  

**Rationale:**  
Reducing lorry capacity decreases batching effects, allowing products to be shipped more frequently and reducing waiting time in finished goods storage.

**Expected Impact:**  
Significant reduction in average lead time and improved customer responsiveness.

**Trade-off:**  
Reduced throughput due to smaller shipment sizes and increased logistics frequency.

---


## 3. Results

### 3.1 KPI Comparison

| KPI | Baseline | Optimized |
|----|----------|-----------|
| Throughput (orders/hour) | 4.44 | 1.90 |
| Average Lead Time (hours) | 28.80 | 1.27 |
| Maximum Lead Time (hours) | 56.16 | 8.27 |
| Machine A Utilization | 37% | 16% |
| Machine B Utilization | 52% | 23% |
| Machine C Utilization | 30% | 13% |
| Avg Products per Shipment | 20 | 10 |

---

### 3.2 Interpretation of Results

The optimized configuration achieved a dramatic reduction in average lead time (approximately 95%), indicating that waiting and batching effects were largely eliminated. However, throughput decreased due to smaller shipment sizes, demonstrating a clear trade-off between responsiveness and production volume.

Machine utilization decreased as congestion was removed, resulting in a fast-flowing but lightly loaded system. Buffer utilization stabilized at healthy, non-saturated levels, confirming improved decoupling and flow consistency.

---

## 4. Resiliency Assessment

### 4.1 Model Assumptions

The simulation makes several simplifying assumptions that may not hold in a real factory:

- No quality defects or rework  
- No labor constraints or shift changes  
- Deterministic repair times  
- Single outbound logistics resource  

---

### 4.2 Stress Test Analysis

- **Increased machine failure rates:** Larger buffers and storage provide some protection, but lead time would increase.  
- **30% demand increase:** Machine B would likely become the dominant bottleneck, causing rising queues and lead times.  
- **12-hour parts replenishment delay:** Production would eventually halt once warehouse inventory is depleted.  

---

### 4.3 Single Points of Failure

- Outbound logistics (single lorry)  
- Machine B extended downtime  
- Warehouse stock-out  

---

### 4.4 Safety Margins

Safety margins were intentionally added through increased buffers, storage capacity, and replenishment quantity to improve robustness against variability and delays.

---

## 5. Recommendations

### 5.1 Recommended Configuration

A balanced configuration with moderate buffers, increased finished goods storage, and a medium lorry capacity (e.g., 15 units) is recommended to balance throughput and lead time.

---

### 5.2 Implementation Priority

1. Increase finished goods storage  
2. Increase buffer sizes  
3. Adjust shipping batch size  
 
---

### 5.3 Future Opportunities

Future improvements could include:

- Dynamic shipment policies (time-based departures)  
- Parallel logistics resources  
- Preventive maintenance modeling  
- Quality inspection and rework loops  

---

## Appendix

Baseline and optimized simulation outputs, including KPI summaries and visualization plots, are provided in the `submission_outputs/` directory.
