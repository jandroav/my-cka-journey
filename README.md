# my-cka-journey

## Prepare your exam

* [Certified Kubernetes Administrator (CKA) - A Cloud **Guru**](https://learn.acloud.guru/course/certified-kubernetes-administrator/)
* [Certified Kubernetes Administrator (CKA) with Practice Tests - Udemy](https://cloudbees.udemy.com/course/certified-kubernetes-administrator-with-practice-tests)
* Do 2–3 times the following [CKAD-exercises](https://github.com/dgkanatsios/CKAD-exercises)
* Do the [CKAD series with scenarios](https://codeburst.io/kubernetes-ckad-weekly-challenges-overview-and-tips-7282b36a2681) on Medium
* Do the [CKA series with scenarios](https://killercoda.com/killer-shell-cka)
* Do the [chadmcrowell/CKA-Exercises](https://github.com/chadmcrowell/CKA-Exercises)
* Do the [moabukar/CKA-Exercises](https://github.com/moabukar/CKA-Exercises)
* Do the [alijahnas/CKA-practice-exercises](https://github.com/alijahnas/CKA-practice-exercises)
* Do the [Certified Kubernetes Administrator (CKA) Crash Course](https://github.com/bmuschko/cka-crash-course)


## Exam tips

1. Set the following before starting the examn:
    ```bash
    alias k=kubectl                         # will already be pre-configured
    export do="--dry-run=client -o yaml"    # k get pod x $do
    export now="--force --grace-period 0"   # k delete pod x $now
    ```
2. Create the file ~/.vimrc with the following content:

   ```bash
   set tabstop=2
   set expandtab
   set shiftwidth=2
   ```

3. Open 2 tabs with the following:
   
   - [Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
   - [Kubernetes Official Documentation index](https://kubernetes.io/docs)
  
4. When you have to fix a component (like kubelet) in one cluster, just check how its setup on another node in the same or even another cluster. You can copy config files over etc.

## Vim tips

- `:set number` followed by Enter to toggle line numbers
- `:set paste` to paste fromated `yaml`
- Copy&paste
  - Mark lines: `Esc+V` (then arrow keys)
  - Copy marked lines: `y`
  - Cut marked lines: `d`
  - Past lines: `p` or `P`

## Fast commands

* Get pod base `yaml`: `k -n namespace run p2-pod --image=nginx:1.21.3-alpine $do > p2-pod.yaml`
* Get pod labels: `k -n namespace get pod -L app`
* Create service pod: `k -n namespace expose pod p2-pod --name p2-service --port 3000 --target-port 80`
* Filter by labels: `k get svc,ep -l app=check-ip`


## Important paths & config files

* `/etc/systemd/system/kubelet.service.d/10-kubeadm.conf`: Kubelete config file
* `/etc/kubernetes/pki`: Certificates
* `cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep etcd`: Check `etcd` config
* Change `CIDR` -> `/etc/kubernetes/manifests/kube-apiserver.yaml` and `/etc/kubernetes/manifests/kube-controller-manager.yaml` (at `--service-cluster-ip-range`)


## Usefull stuff

### Schedule pods in Control Plane nodes ([tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/))

```yaml
spec:
  tolerations:                            
  - effect: NoSchedule                    
    key: node-role.kubernetes.io/master   
```

### Confirm `k8s` services are running

* `crictl ps | grep kube-proxy`

* `iptables-save | grep p2-service`


