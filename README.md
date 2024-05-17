# zeevo.nifi

Deploy a NiFi cluster.

## Requirements

This role depends on `docker` being available on hosts.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`)

```yml
# Common variables
nifi_name: nifi # Should be changed to your NiFi's FQDN
nifi_user: nifi # This user MUST have UID 1000
nifi_image: apache/nifi:2.0.0-M2
nifi_home_dir: /home/nifi # NiFi's will store all of its data here by default
nifi_nar_extensions_dir: /data/nar_extensions # Custom NAR files

# Security
nifi_keystore: keystore.jks # Filename of your keystore on the Host machine
nifi_truststore: truststore.jks # Filename of your truststore on the Host machine
nifi_security_keystore_password: keystorepass
nifi_security_keystore_type: JKS
nifi_security_truststore_password: truststorepass
nifi_security_truststore_type: JKS

# Cluster mode
nifi_cluster_is_node: true # Enable or disable clustered mode
nifi_nodes: # Cluster members. Without this, NiFi nodes will not be able to communicate with eachother
  - dn: CN=ec2-100-25-205-207.compute-1.amazonaws.com, OU=NIFI

# Single user mode
nifi_single_user_mode: false
nifi_single_user_credentials_username: nifi
nifi_single_user_credentials_password: nifinifinifinifi

# Zookeeper
nifi_zookeeper_connect_string: my-zookeeper-server.com:2181 # your Zookeeper connection string.
```

## Dependencies

Deploy a zookeeper using the [zeevo.zookeeper](https://github.com/zeevo/ansible-role-zookeeper) companion Role.

```yml
# Zookeeper playbook
- name: zookeeper
  vars:
    zookeeper_user: admin
    docker_users:
      - admin
  hosts: zookeeper
  become: true
  roles:
    - zeevo.docker
    - zeevo.zookeeper
```

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yml
# Playbook `nifi.yml`
- name: docker
  hosts: all
  become: true
  vars:
    docker_users:
      - admin # Should be the user that will run NiFi and Zookeeper
  roles:
    - zeevo.docker

- name: zookeeper
  hosts: zookeeper
  become: true
  vars:
    zookeeper_user: admin
  roles:
    - zeevo.zookeeper

- name: nifi
  hosts: nifi
  become: true
  vars:
    nifi_user: admin
  roles:
    - zeevo.nifi
```

## License

MIT

## Author Information

This role was created by and is maintained by [Zeevo](https://zeevo.io).
