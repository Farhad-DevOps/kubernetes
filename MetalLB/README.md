<h2>MetalLB(Load Balancer)on Kubernetes</h2>

**Table of Contents**

- [What is MetalLB Load Balancers in Kubernetes?](#what-is-metallb-in-kubernetes)
- [Metallb Installation Steps By Steps](#metallb-installation-steps-by-steps)
- [Steps 1: Enable strict ARP mode](#steps-1-enable-strict-arp-mode)
- [Steps 2: Namespace + MetalLB manifest apply.](#steps-2-Namespace-+-MetalLB-manifest-apply)
- [Steps 3: Configuration for to advertise the IP Pool](#steps-3-configuration-for-to-advertise-the-ip-pool)
- [Steps 4: Advertise the IP Address Pool](#steps-4-advertise-the-ip-address-pool)
- [Steps 5: Expose server via LoadBalancer](#steps-5-expose-server-via-loadbalancer)

## what is metallb in kubernetes?
MetalLB is a network load balancer for Kubernetes applications running on bare metal hosts. MetalLB lets you use Kubernetes LoadBalancer services, which traditionally use a cloud provider network load balancer, in a bare metal environment.
MetalLB provides external IP to Kubernetes.

## Metallb Installation Steps By Steps

##⚠️ Important Reminder (MetalLB order matters)

	## When installing MetalLB, it is best to follow this sequence:
	## MetalLB install করলে এই sequence follow করাই best:
	
1️⃣ manifests apply
2️⃣ memberlist secret create 
3️⃣ IPAddressPool + L2Advertisement apply

## Steps 1: Enable strict ARP mode?

`kubectl edit configmap -n kube-system kube-proxy`

```bash
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: "ipvs"
ipvs:
  strictARP: true
```

## Steps 2: Namespace + MetalLB manifest apply.
On Master node:

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.15.3/config/manifests/metallb-native.yaml
```
Verify:

```bash
kubectl get ns metallb-system
```
MetalLB pods check:

```bash
kubectl get pods -n metallb-system
```
Expected:

`controller-xxxxx   1/1 Running`
`speaker-xxxxx      1/1 Running`

## Steps 3: Configuration for to advertise the IP Pool

`nano metallb-pool.yaml`

```bash
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: dhcp1-pool
  namespace: metallb-system
spec:
  addresses:
  - 192.168.244.190-192.168.244.220
```
```bash
kubectl apply -f metallb-pool.yaml
```

## Steps 4: Advertise the IP Address Pool

` nano metallb-pool-advertise.yaml`

```bash
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: metallb-dhcp
  namespace: metallb-system
spec:
  ipAddressPools:
  - dhcp1-pool
```

```bash
kubectl apply -f metallb-pool-advertise.yaml
```

## Steps 5: Expose server via LoadBalancer

```bash
kubectl create deployment nginx-dep --image nginx --port 80
```

```bash
kubectl expose deployment nginx-dep --type LoadBalancer --name nginx-svc
```

```bash
kubectl get svc
```

---

🎉🎉🎉 Congratulations you have successfull install MetalLB!!! 🎉🎉🎉
:-----------------:
