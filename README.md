# NFS-exporter container with a file

This container exports /exports with index.html in it via NFS. Based on
../exports. Since some Linux kernels have issues running NFSv4 daemons in containers,
only NFSv3 is opened in this container.

Based on `gcr.io/google-samples/nfs-server`

Built using quay.io: [kingdonb/nfs-data](//quay.io/kingdonb/nfs-data)

