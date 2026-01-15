<h1>How Metrics Server Works (Architecture)</h1>
<h3>High-level flow</h3>

+-----------+       +-----------+       +----------------+
|   Pod     |       |  Kubelet  |       | Metrics Server |
| (App)     | --->  | /metrics  | --->  |                |
+-----------+       +-----------+       +----------------+
                                               |
                                               v
                                       Kubernetes API
                                               |
                                               v
                                     kubectl / HPA

<p>Metrics Server is a scalable, efficient source of container resource metrics for Kubernetes built-in autoscaling 
pipelines.Metrics Server collects resource metrics from Kubelets and exposes them in Kubernetes apiserver through 
Metrics API.These metrics are then used by Horizontal Pod Autoscaler.</p>

<h3>Metrics Server offers:</h3>

<p>
=> A single deployment that works on most clusters (see Requirements)
=>Fast autoscaling, collecting metrics every 15 seconds.
=>Resource efficiency, using 1 mili core of CPU and 2 MB of memory for each node in a cluster.
=>Scalable support up to 5,000 node clusters.</p>
