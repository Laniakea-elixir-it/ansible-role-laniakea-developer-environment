Laniakea Developer Environment
==============================

Ansible role to configure Laniakea Developer Environment. This DE has the purpose of helping Laniakea developers in testing tools and their configuration for Laniakea and its services development. This role can install these tools in an Ubuntu machine (currently it has been tested on Ubuntu 20.04):
* Code Server: code-server is installed and some extensions are added for Python, C++, Ansible and Terraform. Pip and Ansible are also installed if Python and Ansible extensions are added. 
* Nextflow: Nextflow binary is downloaded and put in the user PATH. If both Nextflow and Code Server are installed, the [Code Server extension for Nextflow](https://github.com/nextflow-io/vscode-language-nextflow) is built and added to Code Server.
* Conda: Conda is installed, a default conda environment is created with a defineable set of Python packages installed.
* Docker: Docker is installed with the image directory in the external storage mountpoint.
* NGINX.

Each tool is optional, and can be installed by setting to `true` its relative variable (see below). Also, Code Server, Conda and Docker are configured to write data in the external storage mountpoint.


Role Variables
--------------

## VM variables
| Variable       | Description                      | Default                |
| -------------- | -------------------------------- | ---------------------- |
| export_dir | External volume mountpoint | /export |
| user | VM user | ubuntu |

## Code Server variables
| Variable       | Description                      | Default                |
| -------------- | -------------------------------- | ---------------------- |
| install_code_server | If set to true, Code Server is installed | false |
| code_server.password | Password to access Code Server | password |
| code_server.user | User to access Code Server | {{ user }} |
| code_server.host | Host | 0.0.0.0 |
| code_server.port | Port where Code Server is exposed | 48888 |
| code_server.data_dir | Data directory of Code Server | "{{ export_dir }}/code-server" |
| code_server.work_dir | Working directory of Code Server | "{{ export_dir }}/code-server/workdir" |
| code_server.env.SERVICE_URL | Extensions marketplace URL | https://open-vsx.org/vscode/gallery |
| code_server.env.ITEM_URL | Item URL for marketplace | https://open-vsx.org/vscode/item |
| code_server.extensions | List of code server extensions installed |  |

## Nextflow variables
| Variable       | Description                      | Default                |
| -------------- | -------------------------------- | ---------------------- |
| install_nextflow | If set to true, Nextflow is installed | false |
| nextflow_dir | Directory where Nextflow bin is installed | /usr/local/bin |
| node_gpg_key_url | Node.JS gpg key URL | https://deb.nodesource.com/gpgkey/nodesource.gpg.key |
| node_repo | Deb Node.JS repository | deb https://deb.nodesource.com/node_14.x focal main |
| nextflow_extensions_repo_dest | Directory where the repo of Code Server extension for Nextflow is cloned | /tmp/vscode-language-nextflow |
| nextflow_extension_repo_url | Repository of Code Server extension for Nextflow | https://github.com/nextflow-io/vscode-language-nextflow.git |
| vsix_path | Directory where Code Server extension for Nextflow is built | /tmp/nextflow.vsix |

## Conda variables
| Variable       | Description                      | Default                |
| -------------- | -------------------------------- | ---------------------- |
| install_conda | If set to true, Conda is installed | false |
| conda_gpg_key_url | Conda gpg key URL | https://repo.anaconda.com/pkgs/misc/gpgkeys/anaconda.asc |
| conda_repo | Deb Conda repository | deb [arch=amd64] https://repo.anaconda.com/pkgs/misc/debrepo/conda stable main |
| conda_script_path | Path of the Conda script installed by the Deb package | /etc/profile.d/conda.sh |
| conda_bin | Path of the conda binary installed by the Deb package | /opt/conda/condabin/conda |
| conda_envs_dir | Directory where conda environments are created | "{{ export_dir }}/conda_envs" |
| new_conda_env_name | Name of the default conda environment created by the role | default |
| new_conda_env_dir | Directory of the default conda env created by the role | "{{ conda_envs_dir }}/{{ new_conda_env_name }}" |
| new_conda_env_packages | Packages installed in the default conda env | [] |
| python_version | Python version for the default conda env | 3.7 |

## Docker variables
| Variable       | Description                      | Default                |
| -------------- | -------------------------------- | ---------------------- |
| install_docker| If set to true, Docker is installed | false |
| docker.users | User running Docker | {{ user }} |
| docker.install_compose | If set to true, Docker compose is installed | false |
| docker_dir | Directory where Docker images are downloaded | {{ export_dir }}/docker-images |

## NGINX variables
| Variable       | Description                      | Default                |
| -------------- | -------------------------------- | ---------------------- |
| install_nginx | If set to true, NGINX is installed | false |



Example Playbook
----------------

```yaml
---
# Install Code Server with the default extensions and Nextflow with its extensions
- name: Developer environment
  hosts: all
  roles:
    - role: laniakea.developer_environment
      vars:
        install_code_server: true
        install_nextflow: true
```


Author Information
------------------

[Daniele Colombo](https://github.com/dacolombo)