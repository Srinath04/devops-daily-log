# 📅 2025-08-08

## 🔧 Activities

1. Deployed a simple web app based on python using Docker on a local Ubuntu machine.
2. Deployed the same app as a pod in Minikube.
3. Exposed the pod using a Kubernetes service.
4. Accessed both deployments via a web browser and checked app functionality.
5. Compared response times between Docker and Minikube using `curl`.

## 🔍 Key Learnings

- Docker provides direct localhost access with minimal response time -- ideal for quick, standalone application runs and testing on a single machine.
- Minikube requires service exposure (NodePort), and access is through its internal VM IP.
- Minikube introduces slightly higher latency due to its internal abstraction and networking layers -- this trade-off comes with the benefit of simulating a scalable, production-like Kubernetes environment.
- Command line tools like are good for timing and analysis of performance metrics.

## 🧪 Commands / Configs

# Test Performance
curl -o /dev/null -s -w "DNS Lookup: %{time_namelookup}s\nConnect: %{time_connect}s\nTTFB: %{time_starttransfer}s\nTotal Time: %{time_total}s\n" http://localhost:80

time curl -s http://localhost:80 > /dev/null

