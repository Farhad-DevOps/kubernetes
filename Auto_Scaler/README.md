<h1>How Kubernetes Autoscaling Configuration</h1>
<h3>What is Auto-Scaling</h3>
<p>Scalability is one of the core benefits of Kubernetes (K8s). In order to get the most out of this benefit (and use K8s effectively), you need a solid understanding of how Kubernetes autoscaling works.</p>

 ## What is kubernetes autoscaler
 Autoscaling in Kubernetes refers to the automatic adjustment of resources in response to changes in workload demand. Kubernetes provides several mechanisms for autoscaling: 
- Including Horizontal Pod Autoscaling (HPA)
- Vertical Pod Autoscaler (VPA)
- Cluster Autoscaler

## Horizontal Pod Autoscaling (HPA) Configuration Steps
We’ll work through the following steps one-by-one
1. Create a k8s cluster
2. Install the Metrics Server
3. Deploy a sample application
4. Apply Horizontal Pod Autoscaler
5. Increase load to the Application
6. Monitor Events



<h3>Metrics Server offers:</h3>

<p>
=> A single deployment that works on most clusters (see Requirements). <br>
=> Fast autoscaling, collecting metrics every 15 seconds.<br>
=> Resource efficiency, using 1 mili core of CPU and 2 MB of memory for each node in a cluster.<br>
=> Scalable support up to 5,000 node clusters.</p>
