
### **Daily Progress – Etcd Backup and Restore**

**Topic Covered:**

- **Etcd Backup and Restore in Kubernetes**
    
**Key Learnings:**

- Understood the **role of etcd** as Kubernetes’ key-value store that holds all cluster configuration and state data.
    
- Practiced how to **take an etcd snapshot**:
    
    `ETCDCTL_API=3 etcdctl snapshot save /tmp/snapshot.db \ --endpoints=https://127.0.0.1:2379 \ --cacert=/etc/kubernetes/pki/etcd/ca.crt \ --cert=/etc/kubernetes/pki/etcd/server.crt \ --key=/etc/kubernetes/pki/etcd/server.key`
    
- Verified snapshot integrity using:
    
    `ETCDCTL_API=3 etcdctl snapshot status /tmp/snapshot.db`
    
- Restored from snapshot with:
    
    `ETCDCTL_API=3 etcdctl snapshot restore /tmp/snapshot.db \ --data-dir /var/lib/etcd-from-backup`
    
- Updated the **static pod manifest** `/etc/kubernetes/manifests/etcd.yaml`  
    to point to the new `data-dir` for the restored state, ensuring cluster comes back online cleanly.
    
- Validated cluster health post-restore using `kubectl get nodes` and `etcdctl endpoint health`.
    
**Additional Notes:**

- Even though **etcd backup and restore** is **not part of the latest CKA syllabus**, practiced it for the purpose of **backing up critical configurations and cluster data**.
    
- though **Git like version control** is the **preferred method** for **safe code and configuration rescue**,  
    but in **cluster recovery scenarios**, the **inbuilt etcd backup mechanism** is extremely useful and sometimes the only way to restore operational state.
