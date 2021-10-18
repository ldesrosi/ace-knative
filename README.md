# ace-knative

This repository is largely based on a subset of the scripts from https://github.com/ot4i/knapp-connect and from the full documentation available here:  
https://ot4i.github.io/knapp-connect/  

It is a simplified version to focus on testing ACE flows with k-native serving.    

To get started:  
1. Create namespace using `oc apply -f namespace.yaml`  
2. Create the `cp.icr.io` secret following this command:  
```  
oc create secret docker-registry \
  ibm-entitlement-key \
  --docker-username=cp \
  --docker-password=<entitlement-key> \
  --docker-server=cp.icr.io
``` 
Note that the entitlement key can be retrieved [here](https://myibm.ibm.com/products-services/containerlibrary)

3. Tag the ace image
```  
oc tag cp.icr.io/cp/appc/ace-server-prod@sha256:04bc376391a00ff1923d9122f93911b0f8e9700c7dda132f24676e383c0283cc ace-server-prod:latest-amd64  
```  
4. Start the build  
```  
oc start-build ace-build --from-file ./bars/HTTPEcho.bar  
```  
3. Setup the k-native `serving` using these two links:  
* https://docs.openshift.com/container-platform/4.7/serverless/admin_guide/install-serverless-operator.html  
*  https://docs.openshift.com/container-platform/4.7/serverless/admin_guide/installing-knative-serving.html  
  
4. Create the build config using `oc apply -f build/`  
5. Create the k-native declaration for the HTTPEcho ACE service: `oc apply -f integration-server-knative-service.yaml`
6. Get the ACE k-native endpoint: `oc get ksvc ace-echo --template={{.status.url}}`  

Service can be tested using:
```
time curl http://<<ace-knative-endpoint-from-step-6>>/Echo
```