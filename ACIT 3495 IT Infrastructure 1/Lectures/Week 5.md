### top 3 container orchestrators
- docker swam
- kubernetes
- mesos

### container orchestration layer
- service management
- scheduling
- resource management
- kubernetes acts kinda like the OS
	- regardless of the hardware, it just knows how to handle these
	- put kubernetes on a cluster of notes

### what is kubernetes?
- open source orchesteration system used for
	- deployment of containerized applications
	- scaling of containerized applications
	- management of containerized applications 

### how does it work?
#### 1. **Cluster**
A Kubernetes cluster =
- **Control Plane** (the brain)
- **Worker Nodes** (where containers actually run)

#### 2. **Control Plane (Manager)**
- Decides _what should happen_.
- Key parts:
	- **API Server** – entry point (kubectl talks to this)
	- **Scheduler** – decides which node runs a pod
	- **Controller Manager** – keeps things in the desired state
	- **etcd** – key-value store holding cluster state
- You usually don’t touch these directly.

#### 3. **Nodes**
- Worker machines (VMs or physical).
- Each node runs:
	- **kubelet** – talks to control plane
	- **container runtime** (Docker/containerd)
	- **kube-proxy** – networking
	    
#### Pods (Smallest Unit)
- A **Pod**:
	- Is the smallest deployable unit
	- Usually contains **one container**
	- Shares network + storage inside the pod
- You don’t scale containers — you scale **pods**.
- each pod should only do 1 thing

#### Deployments
A **Deployment**:
- Manages Pods
- Handles:
    - Scaling
    - Updates
    - Rollbacks
    - Self-healing

#### Services (Networking)
- Pods get recreated → their IPs change.
- A **Service**:
	- Provides a **stable IP / DNS name**
	- Load-balances traffic across pods
- Types:
	- ClusterIP (internal)
	- NodePort
	- LoadBalancer
	    
#### Storage (Persistence)
- Containers are ephemeral.
- Kubernetes uses:
	- **PersistentVolume (PV)**
	- **PersistentVolumeClaim (PVC)**
- This lets data survive pod restarts (e.g., databases).
