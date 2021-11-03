## Repository for demo Submariner and RHACM

All of this commands needs to be performed in the ACM Hub cluster to deploy through GitOps. All the resources will be created on the hub cluster, the placement rules and the labels you apply on your clusters will determine were each part of the application is deployed. The deployment is based on the next labels: app=guestbook-app, db-master=redis-master-app, db-slave=redis-slave-app.

* Deploy the GuestBook App in cluster Managed 1

```
oc apply -k guestbook-app/acm-resources
```

* Deploy the Redis Master App in Cluster Managed 1

```
oc apply -k redis-master-app/acm-resources
```

* Deploy the Redis Slave App in Cluster Managed 2

```
oc apply -k redis-slave-app/acm-resources
```
