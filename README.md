# k8s-prometheus-grafana

Config prometheus:
kubectl apply -f node-exporter.yaml
kubectl apply -f prometheus/configmap.yaml -f prometheus/prometheus.deploy.yml -f prometheus/prometheus.svc.yml -f prometheus/rbac-setup.yaml 

Config NFS before grafana:
1. Install NFS service
sudo apt install nfs-kernel-server

2. Add the following line in /etc/exports
/data *(rw,sync,no_subtree_check,no_root_squash)

3. Create share folder
sudo mkdir -p /data/pv/grafana

4. Restart NFS service
sudo service nfs-kernel-server restart

5. Change owner for grafana
sudo chown -R 472:472 /data/pv

Config grafana:
kubectl apply -f 01-1-grafana-pvc.yaml -f  01-2-grafana-pv.yaml -f 01-3-grafana-statefulset.yaml -f 01-4-grafana-svc.yaml

Check pod and service status:
kubectl get pod,svc --all-namespaces


[Kubernetes+Prometheus+Grafana部署笔记](http://blog.51cto.com/blogger/publish/2160569)
[K8S自己动手系列 - 2.5 - StatefulSet & Grafana](https://yq.aliyun.com/articles/706604)
[ubuntu 16.04 安装 nfs](https://www.cnblogs.com/tracey/p/8506334.html)
