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

## Troubleshooting

When you deploy this demo on ocp, you may get the following errors
```
AH00558: apache2: Could not reliably determine the server's fully qualified domain name ...
(13)Permission denied: AH00072: make_sock: could not bind to address [::]:80
(13)Permission denied: AH00072: make_sock: could not bind to address 0.0.0.0:80
no listening sockets available, shutting down
AH00015: Unable to open logs
```

you need apply a `scc` for this demo by running the following command, e.g.

```yaml
cat << EOF | oc apply -f -
apiVersion: security.openshift.io/v1
kind: SecurityContextConstraints
metadata:
  name: guestbook-demo-scc
allowHostDirVolumePlugin: true
allowHostIPC: true
allowHostNetwork: true
allowHostPID: true
allowHostPorts: true
allowPrivilegeEscalation: true
allowPrivilegedContainer: true
allowedCapabilities:
- '*'
allowedUnsafeSysctls:
- '*'
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
groups:
- system:cluster-admins
- system:nodes
- system:masters
priority: 1
readOnlyRootFilesystem: false
requiredDropCapabilities: []
runAsUser:
  type: RunAsAny
seLinuxContext:
  type: RunAsAny
seccompProfiles:
- '*'
supplementalGroups:
  type: RunAsAny
users:
- system:serviceaccount:guestbook:default
volumes:
- '*'
EOF
