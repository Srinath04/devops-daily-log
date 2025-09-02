
# Today's Kubernetes Learnings Documentation

## Ingress Troubleshooting

* **Editing Ingress Hosts**: Learned how to identify and fix issues with the `host` field in Ingress configuration. Ensuring the correct DNS entry matches the Ingress resource to route traffic properly.
* **Useful when application endpoints are not resolving due to mismatched hostnames in the Ingress manifest.

## Vertical Pod Autoscaler (VPA)

* **Initial Setup**: Explored enabling and configuring VPA in Kubernetes clusters to automatically adjust CPU and memory requests/limits.
* **Modes of Operation**:

  * **Initial**: Sets resource requests when the pod starts.
  * **Auto**: Dynamically updates resource requests for running pods.

## OOMKilled Errors

  * Observed how improper resource allocation leads to `OOMKilled` pod terminations.
  * Understood the importance of VPA in mitigating such errors by adjusting memory automatically.
* **Real-world use case**: Ensures applications scale vertically in resource-constrained environments, preventing downtime due to memory exhaustion.

## Key Takeaways

* Correctly configuring **Ingress hostnames** ensures proper routing of external traffic.
* VPA helps in **optimizing pod resource usage** and prevents **OOMKilled errors** when applications exceed their memory limits.
* Practical troubleshooting involved:

  * Editing Ingress YAMLs.
  * Observing pod events/logs for OOMKilled.
  * Experimenting with VPA modes (`Initial` vs `Auto`).
