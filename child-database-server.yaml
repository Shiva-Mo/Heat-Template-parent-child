heat_template_version: 2015-10-15
description: Launch a basic instance with ubuntu-trusty image and a poolmember for the database server

parameters:

  name:
    type: string
    description: Name of the instance

  key_name:
    type: string
    description: SSH key to inject into the instance
    default: mykey    

  image:
    type: string
    description: Image to deploy the instance from
    default: ubuntu-trusty-14.04_0.0.1

  flavor:
    type: string
    description: Flavor to use for the instance
    default: cb1.medium

  network:
    type: string
    description: The network to connect the instance to the private network

  subnet:
    type: string 
    description: The subnet belonging to the private network

  security_group:
    type: string
    description: List of security groups to attach to the instance

  pool:
    type: string
    description: The load balancer file server pool

  port:
    type: number
    description: Port used by the load balancer database server

resources:

  DB_server:
    type: OS::Nova::Server
    properties:
      name: { get_param: name }
      key_name: { get_param: key_name }
      image: { get_param: image }
      flavor: { get_param: flavor }
      security_groups: 
        - { get_param: security_group }
      networks:
        - network: { get_param: network }
      user_data: |
        #!/bin/bash
        apt-get update
        apt-get install -y nginx
        uname -n > /usr/share/nginx/html/index.html     
########### you can configure the nginx as your desire(e.g. listen to your desired port number)at the bellowing path 
########### /etc/nginx/sites-enabled/default then for applying changes use sudo service nginx restart

  member_pool_DBserver:
    type: OS::Neutron::LBaaS::PoolMember
    properties:
      pool: { get_param: pool }
      address: { get_attr: [ DB_server, first_address ] }
      protocol_port: { get_param: port }
      subnet: { get_param: subnet }

outputs:

  instance_ip1_DBserver:
    value: { get_attr: [ DB_server, first_address ] }

  instance_name1_DBserver:
    value: { get_attr: [ DB_server, name ] }
    
  instance_network1_DBserver:
    value: { get_attr: [ DB_server, networks ] }
