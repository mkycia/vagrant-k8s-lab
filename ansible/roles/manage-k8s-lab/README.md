Role Name
=========

Manage k8s-lab. 
Install docker, minikube, kind etc. and tools like buildspack, skaffold etc.

Requirements
------------

RHEL 7 based linux box i.e CentOS 7, OL 7 etc.

Role Variables
--------------

Boolean variables to define what to install.
Other variables with specific tools versions and some basics parameters.

pre_config: true

docker: 
  install: true

docker_compose:
  version: "1.27.4"

minikube:
  install: true
  start: true
  memory: "4096"
  cpus: "2"
  k8s_version: "1.19.6"

kind:
  install: true
  create_cluster: true

k8s_tools:
  install: true
  pack_install: true
  skaffold_install: true
  octant_install: true


Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

    - hosts: localhost,all
      become: true
      user: vagrant

      vars:
      - pre_config: true

      - docker: 
          install: true

      - minikube:
          install: false
          start: false
          memory: "4096"
          cpus: "2"
          k8s_version: "1.19.6"

      - kind:
          install: false
          create_cluster: false

      - k8s_tools:
          install: true
          pack_install: true
          skaffold_install: true
          octant_install: true


      roles:
        - manage-k8s-lab

License
-------

BSD

Author Information
------------------

Marcin Kycia
