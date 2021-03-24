# Ansible Role: Go

Ansible role for installing [Go](https://golang.org/).

[![CI](https://github.com/PyratLabs/ansible-role-go/actions/workflows/ci.yml/badge.svg)](https://github.com/PyratLabs/ansible-role-go/actions/workflows/ci.yml)

## Requirements

This role has been tested on Ansible 2.7.0+ against the following Linux Distributions:

  - Amazon Linux 2
  - CentOS 8
  - CentOS 7
  - Debian 10
  - Fedora 29
  - Fedora 30
  - Fedora 31
  - Ubuntu 18.04 LTS

## Disclaimer

If you have any problems please create a GitHub issue, I maintain this role in
my spare time so I cannot promise a speedy fix delivery.

## Role Variables


| Variable                     | Description                                                             | Default Value |
|------------------------------|-------------------------------------------------------------------------|---------------|
| `go_version`                 | Use a specific version of go, eg. `1.13.7`. Specify `false` for latest. | `false`       |
| `go_install_os_dependencies` | Allow role to install OS dependencies.                                  | `false`       |
| `go_install_dir`             | Installation directory for go.                                          | `$HOME/bin`   |
| `go_projects_dir`            | Directory to put go projects. Specify `false` to skip.                  | `$HOME/go`    |
| `go_configure_goenv`         | Configure environment variables in user's .bashrc                       | `true`        |
| `go_projects`                | List of go projects to be cloned with `git`. See notes.                 | _NULL_        |

## Dependencies

No dependencies on other roles.

## Example Playbook

Example playbook for installing to single user:

```yaml
- hosts: go_workstation
  roles:
     - { role: xanmanning.go, go_version: 1.13.7 }
```

Example playbook for installing the latest go version globally:

```yaml
---
- hosts: go_workstation
  become: true
  vars:
    go_install_os_dependencies: true
    go_install_dir: /usr/local/bin
    go_projects_dir: /opt/go/projects
    go_configure_goenv: false
  roles:
    - role: xanmanning.go
```

### Note about `go_projects`

This is a list of git repositories to be cloned into the projects directory.
If this is empty, no projects will be cloned.

Below is an example of a project:

```yaml
go_projects:
    - name: gin                                       # Directory name to clone into
      repo: git@github.com:gin-gonic/gin              # Repository to clone
      update_repo: true                               # Always update local copy of repo
      version:  master                                # Check out this version of the repo
      force: false                                    # Discard any existing working copy of the repo
      key_file: "{{ ansible_user_dir }}/.ssh/id_rsa"  # Key file to use to clone the repo
      recursive: true                                 # Include submodules in clone
```

## License

[BSD 3-clause](LICENSE.txt)

## Author Information

[Xan Manning](https://xan.manning.io/)
