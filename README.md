====
## Ansible Package Management

Sometimes for development systems or Jenkins agents there is a need to install multiple versions of a package, without interferring the OS installed version. This is difficult or impossible to do with Yum.

### JDK and Graddle Installs

The jdk and gradle role can be called with the following setup in your ansible playbook yml file.

```
- hosts: "*"
  become: yes

  vars:
    scala_version: "2.11.11"
    sbt_version: "0.13.15.3"
    users: "{{ ops_users }}"

  roles:
    - { role: jdk, tags: jdk, jdk_version: "jdk-8u192", jdk_extension: "-linux-x64.tar.gz" }
    - { role: jdk, tags: jdk, jdk_version: "jdk-8u201", jdk_extension: "-linux-x64.tar.gz" }
    - { role: jdk, tags: jdk, jdk_version: "jdk-11.0.2", jdk_extension: "_linux-x64_bin.tar.gz" }
    - { role: gradle, tags: gradle, gradle_version: "4.6", gradle_sha: "98bd5fd2b30e070517e03c51cbb32beee3e2ee1a84003a5a5d748996d4b1b915" }
    - { role: gradle, tags: gradle, gradle_version: "5.2.1", gradle_sha: "748c33ff8d216736723be4037085b8dc342c6a0f309081acf682c9803e407357" }
```

From the above example, this will install the JDK version 8u192, 8u201 and 11.0.2. In order to use this role you need to update the `jdk_distribution_url` variable in the `jdk/defaults/main.yml`  with the location of your downloaded versions of JDK. I went with this approach as the new JDK download requires a login and I found that I ran into too many issues with trying to authenticate for the downloads on every Ansible run.

For gradle I found that downloading from gradle distribution URL to work without issue. 


## yum-pkgs
This Ansible role is a simple way to group and install different yum packages on AWS Linux instances. It is just a quick way to group specific packages together for different AMI types. 

It will install different package groups based on the passed in variable `packages_<package group>`. For example, if we want to install `git` and some network monitoring packages, include the following in your playbook yml file. 

```
- hosts: "*"
  become: yes

  roles:
    - { role: packages, tags: packages, packages_git: True, packages_network: True, packages_mysql_client: True, ansible_distribution_major_version: "6" }
    - 
```



