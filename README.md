DevOps Practical Exercise
=========

Welcome to HomeAdvisor's DevOps practical exercise! This exercies uses Git, Docker, Ansible and Molecule.

The intent of the exercise is to use Configuration Management (Ansible) to define an end state of a piece of infrastructure (Docker container) and validate 
it using a devlopment and testing framework (Molecule).

This project contains a partial implementation of what HomeAdvisor might vaguely consider a "build node" that could be use for compilation/packaging/deployment.

It is frought with poor architectural decisions, inefficiencies and other darkness. It could use some love to be more efficient and less hack-y.

Practical Exercise Goals
------------------------
The primary goal of this exercise is to foster a discussion about some of the technical choices you made and why. There are many different paths that could be taken to achieve some or all of the following outcomes:
* At a minimum, `molecule converge` should pass successfully
* Ideally `molecule test` (Lint, Converge, [Idempotence](https://docs.ansible.com/ansible/latest/reference_appendices/glossary.html), Verify, Destroy) should pass successfully
* The instance should have the ability to install new features in Docker by opting to use the Docker Edge channel instead of the Docker Stable channel as configured in the Docker CE repo file
* A critical vulnerability has been identified in the current stable version of Docker CE (18.09.1). If opting to use the stable channel, we should upgrade to the at minimum, the version that closes the security loophole as documented in their [release notes](https://docs.docker.com/engine/release-notes/)
* By default, Molecule is configured to use [Pytest](https://docs.pytest.org/en/latest/) and [Testinfra](https://testinfra.readthedocs.io/en/latest/) to run tests against the instance, but this Ansible role lacks many (any) tests. If you are more comfortable with a different testing framework, please feel free to swap those out. Regardless, it would be excellent if we could run tests via `molecule verify` after a `molecule converge` or as part of a larger `molecule test` lifecycle to verify the tasks executed as part of the Ansible run produced the desired output.

While it is not an explicitly stated goal of this exercise, think about and be prepared to discuss how you would validate and deliver potential changes to this repository as part of a CI/CD pipeline.


Software Requirements
------------
* [Python](https://www.python.org/downloads/)
* [Git](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/install/)
* [Ansible](https://docs.ansible.com/ansible/latest/index.html)
* [Molecule](https://molecule.readthedocs.io/en/stable/)
* [Gilt](https://gilt.readthedocs.io/en/latest/)


Ansible Role Explanation
------------------------

The Ansible role currently performs the following actions:
* Installs some system level packages for general tool setup
* Installs the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-bundle.html) via the bundled installer
* Installs the [AWS Corretto JDK](https://docs.aws.amazon.com/corretto/latest/corretto-8-ug/what-is-corretto-8.html)
* Installs [Maven](https://maven.apache.org/)
* Installs Docker CE and configures the daemon.json

Ansible Role Variables
----------------------

Currently, this role has no required variables. Feel free to add role variables if they fit your overall scheme to make this code more production hardened.

Ansible Role Dependencies
-------------------------

This role currently has no dependencies. If dependencies are added, the role is currently configured to use Gilt as a dependency management solution. Feel free to update to a different provider if that makes you more comfortable.

Ansible Example Playbook
------------------------

The [playbook.yml](roles/ha-devops-practical/molecule/default/playbook.yml) defined in the `molecule/default` directory includes a general example playbook structure.
The role can be extended by overriding `defaults/main.yml` directly from the `playbook.yml` in a manner similar to:

    - hosts: servers
      roles:
        - role: ha-devops-practical
          vars:
            super_boring_bool_var: false


Cool, Where Do I Start?
-----------------------

We recommend installing Docker, Git and Python3 first. Once installed, you should be able to:
* Create a virtualenv for this practical
* `source <path_to_your_virtual_env>/bin/activate` for that environment
* `pip install -r requirements.txt` to install the rest of the Python based software requirements (Ansible, Molecule, Gilt, etc)
* Poke around the Documentation above for any software you don't fully understand.
* Start making this tiny world a better place

License
-------

HomeAdvisor Proprietary
