
**Date:** 9 Nov 2025  
**Summary:** Mock test sessions

**Learnings & Clarifications from Today:**

- Understood how **Ingress** works with **Service endpoints** and how to **test an Ingress**.
    
- Clarified **Ingress does not create additional Service endpoints** — it routes external traffic to existing Service endpoints (pods).
    
-  **default NGINX page** and redirect methods.
    
- Explored how to identify **arrays in YAML** using `kubectl explain` or error traces to debug manifests.
	- `mapping values are not allowed here` - likely missed `-` or indent for an array

	- `expected sequence, got mapping` - used `{key: value}` instead of `- key: value`

	- `invalid type for field`- The field expects array/map, not a string
    
- Played with **curl’s equivalent to `tail -f`** for streaming responses (e.g., using `--no-buffer`).
