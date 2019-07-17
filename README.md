Role Name
=========

Create dedicated project , prometheus and grafana deployment in OpenShift cluster

Requirements
------------

```pip install openshift``` 

Role Variables
--------------

```grafana_proxy_sa``` - service account for oauth proxy 

```retention_days``` - remove all data after ```{{ retention_days }}``` 
```router_prefix```  - openshift router  wildcard 

```grafana_app_name``` - add label - ```name: {{ grafana_app_name }}```

```grafana_project_dashboards``` - prometheus will collecting metrics for projects from this list

```registry_prefix``` - docker registry prefix, i.e quay.io

```prometheus_image_name``` - prometheus image name

```prometheus_image_tag``` - prometheus image tag
 
```grafana_image_name```  - grafana image name

```grafana_image_tag```   - grafana image tag (version)

```alertmanager_image_name``` - alert manager image name

```alertmanager_image_tag``` - alert manager image tag (version)

```oauth_proxy_image_name``` - oauth proxy image name
 
```oauth_proxy_image_tag``` - oauth proxy image tag (version)

```storage_class_name``` - storage class name
 
```use_grafana_pv```  - create PVC for grafana pod (if value is ```no``` - use emptyDir)

```use_prometheus_pv``` -  create PVC for prometheus pod (if value is ```no``` - use emptyDir)

```grafana_pvc_name``` - Grafana PVC name
 
```prometheus_pvc_name``` - Prometheus PVC name

```grafana_pvc_size``` - size of grafana PV

```prometheus_pvc_size``` - size of prometheus PV

Also,  resource restrictions for prometheus and grafana pods: 

```
   #Prometheus
   prom_cpu_request: 500m
   prom_cpu_limit: 1
   prom_ram_request: 512Mi
   prom_ram_limit: 1Gi
   #Grafana
   grafana_cpu_request: 500m
   grafana_cpu_limit: 1
   grafana_ram_request: 512Mi
   grafana_ram_limit: 1Gi
   ```   


Example Playbook
----------------

Playbook example:

    - hosts: "{{ project_list }}"
      become: no
      gather_facts: no
      vars_files:
        - vars/clusters.yaml
      environment:
        K8S_AUTH_HOST: "{{ clusters[cluster_name]['k8s_auth_host'] }}"
        K8S_AUTH_API_KEY: "{{ clusters[cluster_name]['k8s_auth_api_key'] }}"
      tasks:
        - include_role:
            name: gap
          vars:
            project_name: "{{ inventory_hostname }}"
            
Example above for usage with particulary cluster inventory, for simpler  enough define variable 
```{{ project_name }}``` ,  something like this:

        - include_role:
            name: gap
          vars:
            project_name: "exist_project_for_gap"



License
-------

BSD

Author Information
------------------

Igor Yurev,  idyurev@gmail.com
