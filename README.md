# 🧪 Canary - Lab  Progressive Delivery Canary Analysis Simulator
Python · NumPy · Pandas · Matplotlib  

CanaryLab is a Python-based simulator for **canary deployments** and **progressive delivery analysis**.  
It models baseline vs. canary traffic, generates latency and error metrics, applies statistical tests, and produces a verdict on whether the canary is safe to promote.  

This project is useful for **learning release strategies, SRE practices, and automated rollout decisions**.

---------------

## ✨ What It Does
- Simulates **baseline vs. canary traffic** (configurable traffic split, e.g., 90/10)  
- Generates **latency and error metrics** with tunable deltas for canary behavior  
- Applies a simplified **Welch’s t-test** for latency comparison  
- Compares error rates to baseline with thresholds  
- Outputs:  
  - **CSV** with sampled traffic events  
  - **PNG** with latency distribution + error rate chart  
  - **Console verdict** (PASS / FAIL + reasons)  

---

## 🛠 Tech & Languages

| Layer        | Tech         | Notes                                         |
|--------------|--------------|-----------------------------------------------|
| Language     | Python 3.10+ | Core simulation + analysis logic              |
| Data         | Pandas       | Aggregation and CSV export                    |
| Math         | NumPy        | Randomized traffic metrics and statistics     |
| Charts       | Matplotlib   | Latency histograms and error rate plots       |
| Runtime      | Colab/Local  | Works in Google Colab or any Python env       |

---

## 🌐 Architecture

**Flow**
1. **TrafficSplit** routes requests to baseline or canary.  
2. **MetricSim** generates latency/error metrics for each request.  
3. **Analyzer** runs t-test + error comparisons.  
4. Verdict (PASS/FAIL) is logged with reasons.  
5. Charts and CSV are exported.  

---

## 📦 Repository Structure

canarylab/
├─ app/
│ ├─ simulator.py # Traffic + metric simulation
│ ├─ analyzer.py # Statistical analysis + verdict
├─ outputs/
│ ├─ canary_samples.csv # Simulated request-level metrics
│ └─ canary_compare.png # Latency & error charts
├─ tests/
│ └─ test_analyzer.py # Unit tests for analyzer logic
├─ requirements.txt
└─ README.md

yaml
Copy code

---

## ▶️ Run in Google Colab

Paste the **single code cell** into Colab and run.  
Then, check outputs in your working directory.  

```python
# Quick demo
analyzer = CanaryAnalyzer(
    split=TrafficSplit(baseline=0.9, canary=0.1),
    sim=MetricSim(base_latency_ms=120, base_error_rate=0.01,
                  canary_delta_lat_ms=18, canary_delta_error=0.006)
)
analyzer.run(steps=6, batch=1200)
result = analyzer.analyze()
print(result)
analyzer.plot("canary_compare.png")
analyzer.history.to_csv("canary_samples.csv", index=False)
Artifacts created:

canary_samples.csv → All traffic samples

canary_compare.png → Latency + error charts

Console → PASS/FAIL verdict with reasons

📊 Sample Outputs
Console verdict

vbnet
Copy code
=== Canary Analysis ===
baseline_latency_ms: 119.8
canary_latency_ms: 138.7
baseline_error_rate: 0.010
canary_error_rate: 0.017
verdict: FAIL
reasons: ['Canary error rate too high', 'Canary latency regression']
canary_samples.csv

is_canary	latency_ms	error	step
False	118.2	0	1
False	130.5	0	1
True	142.3	1	1

canary_compare.png

Panel 1: Latency histogram (baseline vs canary)

Panel 2: Error rate per step (line plot)

🧪 Tests
Run with:

bash
Copy code
pytest -q
Included tests:

t-test math correctness

Error rate comparison logic

PASS/FAIL verdict rules

🐳 Docker
Build and run:

bash
Copy code
docker build -t canarylab:latest .
docker run -it canarylab:latest
☸️ Kubernetes (Optional Extension)
Deploy CanaryLab as a microservice to validate real traffic canaries:

Wrap analyzer in FastAPI

Expose /analyze endpoint for incoming metrics

Run inside your cluster as part of release pipeline

Integrate with Argo Rollouts or Flagger

🔐 Production Notes
Replace synthetic metrics with Prometheus/Datadog real requests

Use full Mann–Whitney U test or Kolmogorov-Smirnov for latency

Integrate into CI/CD pipelines (progressive delivery gates)

Extend scoring: add throughput, saturation, Apdex

👤 Author
Siddharth Raut — DevOps & Cloud Engineer
