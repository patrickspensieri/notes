add a helm repo
`helm repo add stable https://kubernetes-charts.storage.googleapis.com/`

list releases
`helm list`

install a chart
`helm install patrick-aero stable/aerospike -f aerospike-values.yaml`

uninstalling a chart
`helm uninstall patrick-aero`

upgrade a release
`helm upgrade --install patrick-aero --values new-values.yaml stable/aerospike`

see revision history
`helm history patrick-aero`

rollback to first release
`helm rollback patrick-aero 1` for first revision
`helm rollback patrick-aero 0` for last revision


