<h2>MetalLB(Load Balancer)on Kubernetes</h2>

**Table of Contents**

- [What is MetalLB Load Balancers in Kubernetes?](#what-is-metallb-in-kubernetes)
- [MetalLB Installation Steps](#metallb-installation-steps-by-steps)
- [Steps 1: Enable strict ARP mode](#steps-1-enable-strict-arp-mode)
- [Steps 2: Namespace + MetalLB manifest apply](#steps-2-Namespace-+-MetalLB-manifest-apply)
- [Steps 3: Layer 2 Configuration for to advertise the IP Pool](#steps-3-layer-2-configuration-for-to-advertise-the-ip-pool)
- [Steps 4: Advertise the IP Address Pool](#steps-4-advertise-the-ip-address-pool)
- [Steps 4: Expose server via LoadBalancer](#steps-4-expose-server-via-loadbalancer)

## what is metallb in kubernetes?
MetalLB is a network load balancer for Kubernetes applications running on bare metal hosts. MetalLB lets you use Kubernetes LoadBalancer services, which traditionally use a cloud provider network load balancer, in a bare metal environment.
MetalLB provides external IP to Kubernetes.

##metallb-installation-steps-by-steps

##⚠️ Important Reminder (MetalLB order matters)

	## When installing MetalLB, it is best to follow this sequence:
	## MetalLB install করলে এই sequence follow করাই best:
	
1️⃣ manifests apply
2️⃣ memberlist secret create ✅
3️⃣ IPAddressPool + L2Advertisement apply

## Steps 1: Enable strict ARP mode

`kubectl edit configmap -n kube-system kube-proxy`

```bash
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: "ipvs"
ipvs:
  strictARP: true
```

##Steps 2: Namespace + MetalLB manifest apply
On Master node:

`kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.15.3/config/manifests/metallb-native.yaml`
