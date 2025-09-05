
Topic Refresher – Probes(health management), Object Selectors and Dry-Run

## ReadinessProbe vs LivenessProbe

ReadinessProbe: Ensures the container can serve requests before traffic is routed to it.

Determines if a container is ready to accept traffic. If it fails, the pod is marked not ready and removed from the Service’s endpoint list.
Common use: Wait until app initialization completes before receiving traffic.

- Prevents users from hitting an app that is still booting.
- Example: A Java app needing 20 seconds to warm up — readiness delays exposure until it’s ready.

LivenessProbe: Ensures the container stays healthy after it has started.

Determines if a container is alive or stuck. If it fails, Kubernetes restarts the container.
Common use: Recover from deadlocks or unresponsive states.

- Detects stuck processes, deadlocks, or apps that stopped responding.
- Kubernetes will restart unhealthy containers automatically.

### Key Differences

| Probe Type | Purpose Failure | Effect |

|:---|:---:|---:|

| ReadinessProbe | Traffic gating |  Pod temporarily removed from Service |

| LivenessProbe | Container health | Container restarted |

Typical Probes: httpGet, tcpSocket, exec commands.

Example:
```
readinessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10

livenessProbe:
  tcpSocket:
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 20
```

### Real-world Example:
An e-commerce checkout service:
ReadinessProbe ensures it only receives traffic after DB connections are ready.
LivenessProbe restarts it if a memory leak makes the app hang during peak sales.


###  Dry-Run: Client vs Server

#### `--dry-run=client`
- Performs local schema validation using kubectl.
- Fast, but doesn’t reach the API server, so deeper errors (like RBAC, admission webhooks, or cluster policies) aren’t caught.

#### `--dry-run=server`

- Sends the manifest to the API server for full validation (without persisting).
- Catches admission controllers, CRD schema mismatches, or cluster-specific policies.

Example:

``kubectl apply -f pod.yaml --dry-run=server``

###  Command-Line Utility Refresher – cut -d

cut -d specifies a delimiter to split text.

Example:
```
echo "a:b:c" | cut -d ':' -f2
# output: b
```

Useful when piping the required section to file

Example:
```
cat /etc/passwd | cut -d ':' -f1 > users.txt
```

### Default behavior

If not specified, the delimiter defaults to a TAB character.

Example:
```
echo -e "a\tb\tc" | cut -f2
# output: b
```
