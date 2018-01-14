# helm
# information about helms, data type, control structure and other
https://github.com/kubernetes/helm/tree/master/docs/chart_template_guide


* Release: This object describes the release itself. It has several objects inside of it:
    * Release.Name: The release name
    * Release.Time: The time of the release
    * Release.Namespace: The namespace to be released into (if the manifest doesn't override)
    * Release.Service: The name of the releasing service (always Tiller).
    * Release.Revision: The revision number of this release. It begins at 1 and is incremented for each helm upgrade.
    * Release.IsUpgrade: This is set to true if the current operation is an upgrade or rollback.
    * Release.IsInstall: This is set to true if the current operation is an install.
* Values: Values passed into the template from the values.yaml file and from user-supplied files. By default, Values is empty.
* Chart: The contents of the Chart.yaml file. Any data in Chart.yaml will be accessible here. For example {{.Chart.Name}}-{{.Chart.Version}} will print out the mychart-0.1.0.
    * The available fields are listed in the Charts Guide
* Files: This provides access to all non-special files in a chart. While you cannot use it to access templates, you can use it to access other files in the chart. See the section Accessing Files for more.
    * Files.Get is a function for getting a file by name (.Files.Get config.ini)
    * Files.GetBytes is a function for getting the contents of a file as an array of bytes instead of as a string. This is useful for things like images.
* Capabilities: This provides information about what capabilities the Kubernetes cluster supports.
    * Capabilities.APIVersions is a set of versions.
    * Capabilities.APIVersions.Has $version indicates whether a version (batch/v1) is enabled on the cluster.
    * Capabilities.KubeVersion provides a way to look up the Kubernetes version. It has the following values: Major, Minor, GitVersion, GitCommit, GitTreeState, BuildDate, GoVersion, Compiler, and Platform.
    * Capabilities.TillerVersion provides a way to look up the Tiller version. It has the following values: SemVer, GitCommit, and GitTreeState.
* Template: Contains information about the current template that is being executed
    * Name: A namespaced filepath to the current template (e.g. mychart/templates/mytemplate.yaml)
    * BasePath: The namespaced path to the templates directory of the current chart (e.g. mychart/templates).
The values are available to any top-level template. As we will see later, this does not necessarily mean that they will be available everywhere.

```
Step 1) generate your first chart
helm create mychart

step2) dryrun
helm install --dry-run --debug ./hamed-nginx

step3 ) install
helm install --name hamedtestnginx ./hamed-nginx
step 4 ) we can override some of the values
--set service.internalPort=8080

step 5) test
helm ls
helm del --purge hamedtestnginx
kubectl get pods
kubectl get deployments


step 6 )run through lint
helm lint ./hamed-nginx


step 7) package
helm package ./hamed-nginx

step 8 ) helm server
helm serve
Regenerating index. This may take a moment.
Now serving you on 127.0.0.1:8879

helm search local
NAME         	VERSION	DESCRIPTION
local/mychart	0.1.0  	A Helm chart for Kubernetes
helm install --name example4 local/mychart --set service.type=NodePort


step 9 ) helm dependencies
cat > ./mychart/requirements.yaml <<EOF
dependencies:
- name: mariadb
  version: 0.6.0
  repository: https://kubernetes-charts.storage.googleapis.com
EOF

step 10 ) update dep
helm dep update ./mychart

```
