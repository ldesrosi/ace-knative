# ace-knative
Subset of scripts from https://github.com/ot4i/knapp-connect  

Full documentation available here:  
https://ot4i.github.io/knapp-connect/  

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

3. Setup the k-native `serving` using these two links:  
* https://docs.openshift.com/container-platform/4.7/serverless/admin_guide/install-serverless-operator.html  
*  https://docs.openshift.com/container-platform/4.7/serverless/admin_guide/installing-knative-serving.html  
  
4. Create the build config using `oc apply -f build/`  
5. Create the k-native declaration for the HTTPEcho ACE service: `oc apply -f integration-server-knative-service`