
## 🔧 Activity
1. Practiced working with JSON queries using jpath, exploring array slicing, negative indexing, and conditions.
2. Explored JSON structures (arrays vs dictionaries) and how to determine root objects/fields.
3. Experimented with command line JSON manipulation, including printing indices, root field counts, and deleting lines from command output.

## 🔍 Key Learnings
- JSON querying = faster root-cause analysis,ensure data integrity and faster filtering without bloating workflows.

- Negative indexing working:  
'$[-5:]'.events → correctly fetches the last 5 elements,
negative slicing works, but ranges must move left to right, reverse slices like [-1:-5] return empty (not reversed)”.
- Knowing when to treat JSON as an array vs a dictionary helps distinguish between ordered events (like log entries) and unordered attributes (like pod metadata).  

### Valuable in debugging cloud native systems and in pipeline processing.
#### Kubernetes logs & manifests
Checking why a pod is stuck in CrashLoopBackOff:
`` kubectl get pod myapp-xyz -o json | jq '.status.containerStatuses[0].state'``
Here, negative indexing ([-1]) helps get the latest container status instead of scrolling through all restarts.

#### Cloud logs (AWS, GCP, Azure)
#### Example: Filtering the last 5 failed events for a Lambda:
```cat lambda-logs.json | jpath '$[-5:]'.errorMessage```

#### ETL / Data Engineering pipelines
- AI/ML preprocessing often involves filtering large event streams (Kafka, Kinesis, or logs).  
Example: Ingesting IoT sensor data:
``cat sensor.json | jpath '$[-10:]'.temperature``
This lets us quickly fetch the most recent readings for anomaly detection.

- JSON arrays are ordered collections ([ ]), while dictionaries/objects are key–value mappings ({ }). They’re not the same but often used together.

- Quick checks on number of root objects/fields in JSON by parsing with tools like jq or jpath.

- Index printing and line deletion during piping can also be controlled using other common CLI tools.

## Real World Scenarios
- Data Debugging: Parsing JSON logs directly from CLI when debugging APIs, Kubernetes manifests, or system logs.
- Root Field Counting: Validate payloads in CI/CD to catch malformed inputs early.
- CLI Manipulation: Speed up on-call debugging without waiting for dashboards.

## Cost Saving Angle
- Avoids spinning up dashboards or heavy services just for immediate data inspection.
- Minimizes engineering hours during incidents by using fast, local CLI debugging.
- Reduces dependency on third-party JSON parsing/visualization tools.
