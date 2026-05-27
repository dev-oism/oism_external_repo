# oism.external_repo python_packages Role

Download a list of Python packages via `pip` and create a versioned archive for offline distribution.

## Requirements

- Python and `pip` must be available on the target host.
- The target host must have network access to download the requested Python packages.

## Role Variables

The following variables are exposed by this role and can be overridden where needed.

- `python_packages_download_dir`: Directory used to download Python packages before archiving.
- `python_packages_dest_dir`: Directory where the generated tarball and download log are written.
- `python_packages_tar_name`: Name of the output tarball created from the downloaded packages.
- `python_packages_list`: List of Python package names to download.
- `python_packages_my_variable`: Example variable retained for compatibility and demonstration.

### Default values

```yaml
python_packages_download_dir: /tmp/python_packages
python_packages_dest_dir: /srv/repos
python_packages_tar_name: python-packages.tar.gz
python_packages_list:
  - ansible
  - ansible-dev-tools
  - ansible-lint
  - antsibull-changelog
  - antsibull-build
  - antsibull-docs
python_packages_my_variable: default_value
```

## Dependencies

This role has no external role dependencies.

## Example Playbook

```yaml
- name: Download and archive Python packages
  hosts: all
  roles:
    - role: oism.external_repo.python_packages
      vars:
        python_packages_download_dir: /tmp/python_packages
        python_packages_dest_dir: /srv/repos
        python_packages_tar_name: python-packages.tar.gz
        python_packages_list:
          - ansible
          - ansible-lint
          - requests
```

## Role Behavior

- Downloads packages listed in `python_packages_list` using `pip download`.
- Stores downloaded package files in `python_packages_download_dir`.
- Writes a log file in `python_packages_dest_dir`.
- Creates a gzip tarball containing the package downloads.

## Idempotency

The role uses the `creates` option on the `ansible.builtin.command` task to avoid re-downloading packages if the log file already exists. It is idempotent when the destination log file and archive already exist for the same run.

## Argument Specification

This role supports an argument spec file at `meta/argument_specs.yml` to validate role inputs.

```yaml
argument_specs:
  main:
    short_description: Argument specification for the python_packages role.
    options:
      python_packages_download_dir:
        description: Directory to download Python packages.
        type: str
        default: /tmp/python_packages
      python_packages_dest_dir:
        description: Directory to store the tarball and log file.
        type: str
        default: /srv/repos
      python_packages_tar_name:
        description: Name of the generated tarball.
        type: str
        default: python-packages.tar.gz
      python_packages_list:
        description: List of Python packages to download.
        type: list
        elements: str
        default:
          - ansible
          - ansible-dev-tools
          - ansible-lint
          - antsibull-changelog
          - antsibull-build
          - antsibull-docs
```

## License

GPL-2.0-or-later

## Author Information

`oism` role maintainer.
