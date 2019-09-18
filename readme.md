Create the following resources (assuming an RBAC-enabled cluster):

- clusterrole.yml
- serviceaccount.yml
- rolebinding.yml
- metricsgrabber.yml


Run the following command to get the kubelet's metrics from each node:
```
for node in $(kubectl get nodes -o json | jq -r ".items[].metadata.name")
do
  kubectl exec metrics-grabber -- bash -c 'curl -s --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://kubernetes/api/v1/nodes/'$node'/proxy/stats/summary'
done
```
