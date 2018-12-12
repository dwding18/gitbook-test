# FAQ

### How do I get the Port to connect to Dgraph on the minikube cluster to setup the Schema and Metadata?

  
Run the below command and check the NodePort corresponding to port 9080. Here port 30796 will be used.

$ kubectl get services

NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP   PORT\(S\)                         AGE

dgraph-public   LoadBalancer   10.109.53.229   &lt;pending&gt;     5080:30766/TCP,6080:32699/TCP,8080:32038/TCP,9080:30796/TCP,8000:31572/TCP   5h

### Cutlass UI blocked by CORS Policy when a Request is made ?

![CORS Error](../.gitbook/assets/cors-error.png)

Ensure CORS plugin is installed and enabled in the Chrome. 

![](../.gitbook/assets/allow-cors.png)



### Cutlass UI gives Mixed Content error and fails to load data when Request is made?

![Mixed-Content error](../.gitbook/assets/mixed-content-error.png)

When the Mixed Content Error is seen , click on the Shield Icon on the right corner of the browser bar and select "Load Unsafe Scripts" in the dialog that appears as below.

![](../.gitbook/assets/load-unsafe-content-browser.png)

