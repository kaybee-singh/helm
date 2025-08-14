# This repository will discuss basic to advance topics of helm.

Following are important files and directories which are situtated them in a helm chart.

Chart.yaml - It contains metadata of helm like name, version, dependencies etc.
values.yaml - It is the default values file, those values will be referenced by helm templates during helm execution.
templates - It contains go templates which are then rendered into kubernetes/openshift manifests
templates/_helpers.tpl - This file contains the reusable template snippets like names, labels.
charts - It may be dependecies created by helm depndency build
crds - CRDs applied before rendering the template.

Create a helm chart
```bash
helm create httpd-chart
```
Check the created helm chart
```bash
ls -l httpd-chart/
total 8
drwxr-xr-x. 2 root root    6 Aug  5 14:25 charts
-rw-r--r--. 1 root root 1147 Aug  5 14:25 Chart.yaml
drwxr-xr-x. 3 root root  162 Aug  5 14:25 templates
-rw-r--r--. 1 root root 2364 Aug  5 14:25 values.yaml
```
This repository is divided into different sections.


