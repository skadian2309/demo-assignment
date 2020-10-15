1. Created google Kubernetes cluster with three nodes.2. Created one more VM instance with Jmeter to put the loads on application.3. Added host 0.0.0.0 in app.py of AI-flask because by default it was bound with localhost and recreated the docker image.
4. Created Deployment YAML files for both applications.
   nayan-ai-challenge-rails.yaml  
   nayan.yaml5

5. Deployed the above YAML files and checked that applications pods are running successfully or not.
```
kubectl get pods -n demo
NAME                                        READY   STATUS    RESTARTS   AGE
nayan-5f8f8cfd7c-4tstt                      1/1     Running   0          4h58m
nayan-ai-challenge-rails-64f4b5fbc6-r25w2   1/1     Running   0          4h55m
```
6. Created service of both deployment.
```
kubectl get svc -n demo
NAME                       TYPE           CLUSTER-IP   EXTERNAL-IP     PORT(S)          AGE
nayan                      ClusterIP      10.4.1.102   <none>          8080/TCP         5h30m
nayan-ai-challenge-rails   LoadBalancer   10.4.6.43    35.202.189.53   3000:32703/TCP   5h27m
```

8.Scaled the pods  from 1 to 2.
```
kubectl scale --replicas=2 deployment/nayan-ai-challenge-rails -n demo
kubectl scale --replicas=2 deployment/nayan -n demo
```
8. Checked the pods status
```
kubectl get pods -n demo 
NAME                                        READY   STATUS    RESTARTS   AGE
nayan-5f8f8cfd7c-4tstt                      1/1     Running   0          5h13m
nayan-5f8f8cfd7c-fc98g                      1/1     Running   0          2m20s
nayan-ai-challenge-rails-64f4b5fbc6-r25w2   1/1     Running   0          5h10m
nayan-ai-challenge-rails-64f4b5fbc6-x8m5k   1/1     Running   0          112s
```


9. Tested the application end to end,
```
curl  --location --request POST 'http://34.72.138.46:32703/ai/process_image' --form 'image=@env.jso
n.jpg'
{"result":{"label":["tvmonitor: 92.00%"],"BB":[[-3,0,2173,639]]}}
```
10. Created Jmeter script to put the load and started the  load of 100 threads(1 request per second) 
```
sh jmeter.sh -n -t HTTP_Request.jmx
summary =    100 in 00:00:23 =    4.4/s Avg:   224 Min:   201 Max:   480 Err:     0 (0.00%)
Tidying up ...    @ Thu Oct 15 14:13:49 UTC 2020 (1602771229966)
```
