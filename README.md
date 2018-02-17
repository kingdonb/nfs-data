# NFS-exporter container with a file

This container exports /exports with index.html in it via NFS. Based on
../exports. Since some Linux kernels have issues running NFSv4 daemons in containers,
only NFSv3 is opened in this container.

Based on `gcr.io/google-samples/nfs-server`

Built using quay.io: [kingdonb/nfs-data](//quay.io/kingdonb/nfs-data)

To exercise, first get the helm chart installed from here (but it hasn't been
accepted into incubator yet, so you'll still have to get it from me):

```
git clone git@github.com:kingdonb/charts -b nfs-pv charts
cd charts/incubator
helm install --namespace nfs --name nfs nfs-pv-provider
kubectl -n nfs get svc -w
```

Make a note or remember the ClusterIP of the Service that was created here, or
wait until one is assigned.

Then, provision the PV and claim in *this* directory's chart to make it
available to clients.  Edit the values.yaml and write the ClusterIP within.

```
git clone git@github.com:kingdonb/nfs-data
cd nfs-data
$EDITOR values.yaml
helm install --namespace nfs .
```

Finally, mix in one or more of the example clients:

```
kubectl -n nfs create -f example-sh-client/nfs-busybox-rc.yaml
# and/or
kubectl -n nfs create -f example-web-client/nfs-web-rc.yaml
kubectl -n nfs create -f example-web-client/nfs-web-service.yaml
```

One or more pods can connect to a ReadWriteMany PV, so only one PV and PVC called `nfs`
is created, and can serve client podspecs for any number of the examples in this chart!
