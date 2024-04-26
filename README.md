Role Name
=========

This role installs the kubernetes dashboard, nfs csi, awx operator and a corresponding awx instance.

Requirements
------------

A local Kubeconfig (~/.kube/config) and a webserver with context webserver.my.domain/k3s with the following files
* awx-operator helm chart
* nfs-subdir-external-provisioner helm chart
* kubernetes dashboard helm chart

```bash
k3s
└── helm
    ├── awx-operator-2.12.2.tgz
    ├── kubernetes-dashboard-6.0.8.tgz
    └── nfs-subdir-external-provisioner-4.0.18.tgz
```

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

```yaml
---
- name: Install Helm charts
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
# vars for helm charts
    k3s_nfs_server: nfs.my.domain
    k3s_nfs_path: /srv/nfs/k3s
    webserver: 'http://webserver.my.domain/k3s'

    k3s_cert: my.lab.pem
    k3s_cert_key: my.lab.key.pem
    
    k3s_dashboard_helm_chart: kubernetes-dashboard-6.0.8.tgz
    k3s_nfs_provisioner_helm_chart: nfs-subdir-external-provisioner-4.0.18.tgz
    k3s_awx_operator_helm_chart: awx-operator-2.12.2.tgz

    state: present

    instance_name: awx-instance
    
    service_type: ClusterIP
    
    ingress_type: ingress
    ingress_tls_secret: tls-secret
    ingress_hosts: awx-instance.my.domain
    ingress_path: /
    ingress_path_type: Prefix

    ipv6_disabled: true

    projects_persistence: true
    projects_storage_class: standard-sc
    projects_storage_size: 1Gi

  roles:
    - install-dashboard-nfs-awx
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
