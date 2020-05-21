# Ansible role CA Certs

Managing CA Certs.

- [Ansible role CA Certs](#ansible-role-ca-certs)
  - [License](#license)
  - [Author Information](#author-information)
  - [Requirements](#requirements)
  - [Dependencies](#dependencies)
  - [Compatibility](#compatibility)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [Useful shell commands](#useful-shell-commands)
  - [Additional documentation resources](#additional-documentation-resources)
  - [Testing with Molecule](#testing-with-molecule)
  - [CI/CD with Travis CI](#cicd-with-travis-ci)
  - [Useful links](#useful-links)

## License

MIT / BSD

## Author Information

- Made and maintained by: [Kasra Amirsarvari](https://www.linkedin.com/in/caseraw)
- Ansible Galaxy community author: <https://galaxy.ansible.com/caseraw>
- Dockerhub community user: <https://hub.docker.com/u/caseraw>

## Requirements

- Ensure a package manager is available and configured with the correct package sources and repositories.
- Ensure privileged permissions are set for the user executing this role to:
  - Install packages.
  - Update system certificate files and trusts.

## Dependencies

N/A

## Compatibility

Compatible with the following list of operating systems:

- CentOS 7
- CentOS 8
- RHEL 7.x
- RHEL 8.x

## Role Variables

| Variable name | Description |
|---------------|-------------|
| role_cacerts_required_package_list | A per distribution package list. |
| role_cacerts_required_packages | Rendered list of distribution package list. |
| role_cacerts_cert_list | List of CA certificates to deploy or to remove. |

## Example Playbook

```yaml
---
- name: Manage CA certs
  become: True
  gather_facts: True
  tasks:
    - import_role:
        name: ansible_role_cacerts
      vars:
        role_cacerts_required_package_list:
          RedHat_7:
            - ca-certificates
          RedHat_8:
            - ca-certificates
        role_cacerts_required_packages: '{{ role_cacerts_required_package_list[ansible_distribution + "_" + ansible_distribution_major_version] | default([]) }}'
        role_cacerts_cert_list:
          - src: 'example.crt'
            dest: '/etc/pki/ca-trust/source/anchors/example.crt'
            type: file
            state: pesent
          - src: 'https://certs.example.com/example.crt'
            dest: '/etc/pki/ca-trust/source/anchors/example.crt'
            type: url
            validate_certs: False
            state: pesent

...
```

## Useful shell commands

N/A

## Additional documentation resources

N/A

## Testing with Molecule

This role is locally tested with the use of [Molecule](https://molecule.readthedocs.io/en/latest/), the configuration is located at: [molecule/default](molecule/default).  
The Molecule tests are run (using the [docker driver](https://molecule.readthedocs.io/en/latest/configuration.html#docker)) on [Dockerhub images](https://hub.docker.com/u/caseraw) built for this purpose:

- [CentOS](https://hub.docker.com/r/caseraw/ansible-molecule-centos)
- [Fedora](https://hub.docker.com/r/caseraw/ansible-molecule-fedora)

## CI/CD with Travis CI

This role uses [Travis CI](https://travis-ci.org/) to run online tests with the use of [Molecule](https://molecule.readthedocs.io/en/latest/) and pushes notifications to import the role into [Ansible Galaxy](https://galaxy.ansible.com/) once the tests are successful. The Travis CI configuration is located at the root of the Ansible role [.travis.yml](.travis.yml)

## Useful links

- GitHub repository: <https://github.com/Caseraw/ansible_role_cacerts>
- Travis CI build status: <https://travis-ci.org/Caseraw/ansible_role_cacerts>
- Ansible Galaxy role: <https://galaxy.ansible.com/caseraw/ansible_role_cacerts>
