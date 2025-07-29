
### What is HPA (Horizontal Pod Autoscaler)? 

HPA (Horizontal Pod Autoscaler) is a Kubernetes feature that automatically adjusts the number of pod replicas in a Deployment, ReplicaSet, or StatefulSet based on metrics like CPU or memory utilization. It helps applications scale out (add pods) when demand increases and scale in (remove pods) when demand drops, maintaining both performance and cost efficiency. 

The advantages of Horizontal Pod Autoscaler (HPA) in Kubernetes include the following:

#### Automatic scaling based on demand: 
HPA automatically adjusts the number of pods in a deployment or stateful set in response to changing workload metrics like CPU or memory utilization, ensuring applications can handle fluctuating workloads seamlessly.

#### Cost efficiency: 
By scaling down pods when demand is low, HPA helps avoid overprovisioning and reduces cloud infrastructure costs, while scaling up during peak demand to maintain performance.

#### Improved application performance and availability: 
HPA keeps applications responsive by adding replica pods as workload increases, thus enhancing availability and user experience.

#### Reduced manual intervention:
Automating the scaling process frees operators from manually adjusting pod counts, streamlining operations and reducing the chances of human error.

### Commands for HPA:

1) After writting deployment file apply it to create a pod of your deployment.

```
kubectl apply -f deployment.yml 
```
2) Apply service file to expose your application via port. 

```
kubectl -f service.yml
```
3) Apply hpa.yml file to create a autoscaler pod with metrices of CPU.

```
kubectl -f apply hpa.yml
```

All these file created under a specific namespace 
apache. 

```
kubectl get all -n apache
```

Above command shows you all the resources created under apache.

After applying all the files, now we need to generate a load on our application to check if it is scaling automatically based on load and decreasing when load decrease. 

Apply below command to create a pod for load-generator.

```
kubectl run -it --rm load-generator --image=busybox /bin/sh
```
<img width="1704" height="574" alt="image" src="https://github.com/user-attachments/assets/4d3b1d4c-d0f3-4301-ae5d-599585981ef2" />

It will create a pod who generates a load on your application and simultanously increase or decrease the load.

Once pod created, go inside it and run following command to start making requests from this container, run:

```
while true; do wget -q -O- http://php-apache.default.svc.cluster.local; done
```

You can stop this request loop by hitting Ctrl+c.

Try and start increasing the load, then stopping, you should be able to see a difference between the calculated HPA values and the target values predicted by the linear regression.

Check the hpa as well along side to check wheather it is scaling or not.

```
kubectl get pods -n apache
```

```
kubectl get hpa -n apache 
```
<img width="775" height="242" alt="image" src="https://github.com/user-attachments/assets/c78e2b03-e676-4416-b04b-71549552a033" />

To delete all the resources at once type:

```
kubectl delete ns apache
```

To check the logs:

```
kubectl describe pod <pod_name> -n apache
```

```
kubectl logs pod_name -n apache
```

Port Forwarding command

```
kubectl port-forward service/service-name 80:80 --address 127.0.0.1 -n apache
```
