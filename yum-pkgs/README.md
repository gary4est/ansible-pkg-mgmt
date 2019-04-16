====
## Packages Role

This will install different package groups based on the passed in variable `packages_<package group>`. For the bastion host
we want to install `git` and some network monitoring packages. To do this we include the following in the `playbooks/bastion.yml` playbook.

```
roles:
  - { role: packages, packages_git: True, packages_network: True }
```


