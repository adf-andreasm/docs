---
icon: lucide/rocket
---

# Add custom specs repo
There are three repos in place that allow tho manage the clients via git.

1. [The ansible base collection](https://github.com/projectpotos/ansible-collection-potos.base.git) contains role for the first enrollmend and setting up managment triggers. 
  
2. [The ansible potos roles collection](https://github.com/projectpotos/ansible-collection-potos.roles.git) contains roles that allow the configuration of the clients
  
3. [The specs repo](https://github.com/projectpotos/ansible-specs-fedora-test) that is triggert by via the base collection managment and holds the actual host configuration in the group and host vars and deploys it using roles from potos roles collection
  
## Extend configuration
In the `input/config.yml` add the uri for the base collection.

```yaml
collection_source: git+https://github.com/projectpotos/ansible-collection-potos.base.git,main
```

Modify the spec repo information 

```yaml
specs:
  url: https://git.organisation.com/something/specs-repo.git
  branch: main
```

Prepare the spec repo tocken credential

```yaml
credentials:
  specs_token:
    backend: systemd-creds
    name: specs_token
    path: /etc/potos/specs_token.cred
```

Build a new iso using this configuration and install it in a vm


## Setup the ansible pull
On an installed client the access tocken for the specs repo has to be added as follows.

Change the credential setting in the potos configuration 

```yaml
credentials:
  specs_token:
    backend: file
    path: /etc/potos/potos_specs_token.cred
    name: specs_token
````

Write the access token to the file

```bash
echo -n 'glpat-...' > /etc/potos/potos_specs_token.cred
```

The the ansible pull

```
potos-ansible-pull -r daily -v
```
