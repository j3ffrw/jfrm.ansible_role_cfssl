Role Name
=========

Role to manage Cloudflare's CFSSL 

Requirements
------------

* ansible >= 2.14.x
* export ANSIBLE_JINJA2_NATIVE=true

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.


* cfssl_csr_template: a CFSSL CSR template defined as yaml dictionary [cfssl-csr](https://github.com/cloudflare/cfssl/wiki/Creating-a-new-CSR)
* cfssl_container_config: a configuration template when running CFSSL as a api server
  * [signing profile](https://github.com/cloudflare/cfssl/blob/master/doc/cmd/cfssl.txt#L72)
  * Authentication when running as server. [authentication](https://github.com/cloudflare/cfssl/blob/master/doc/cmd/cfssl.txt#L29)
* cfssl_config: a configuration for cfssl running as cli connecting to a cfssl api server
* cfssl_generate_certs: a list of certificates to generate

Dependencies
------------

* community.docker.docker_container

Example Playbook
----------------

    - hosts: localhost
      vars:
        cfssl_generate_certs:
          - cn: "server1.mylocal"
            profile: "server"
            path: /data/cert_mylocal/server1
          - cn: "server2.mylocal"
            profile: "server"
            path: /data/cert_mylocal/server2
    
      pre_tasks:
          - name: Create root and intermediated CA
            ansible.builtin.include_role:
              name: cloudflare-cfssl
              tasks_from: tools/init_ca
              public: true
              apply:
                tags:
                  - init_ca
      roles:
        - name: Start cfssl api server using docker container 
          roles:
            - name: cloudflare-cfssl


License
-------

BSD

Author Information
------------------

https://github.com/j3ffrw
