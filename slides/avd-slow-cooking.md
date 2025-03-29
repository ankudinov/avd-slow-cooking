---
marp: true
theme: default
class: invert
author: Petr Ankudinov
size: 16:9
paginate: true
math: mathjax
style: |
    img[alt~="custom"] {
      float: right;
    }
    .columns {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 1rem;
    }
    .columns3 {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 1rem;
    }
    footer {
      font-size: 14px;
    }
    section::after {
      font-size: 14px;
    }
    img {
      background-color: transparent;
    }
    img[alt~="center"] {
      display: block;
      margin: 0 auto;
    }
---

# AVD Slow Cooking

<style scoped>section {font-size: 26px;}</style>

101 session with tasty recipes 🔪 :cut_of_meat: :carrot:

<!-- Do not add page number on this slide -->
<!--
_paginate: false
-->

![bg right](img/pexels-dapur-melodi-192125-1109197.jpg)

```sh
$ whoami
# Petr Ankudinov, Senior Solutions Engineer
pa #@arista.com
$ date +"%b %Y"                 
Mar 2025
```

<!-- Add footer starting from this slide -->
<!--
footer: '![h:20](https://www.arista.com/assets/images/logo/Arista_Logo.png)'
-->

---

# Agenda

- What is AVD
- Environment
- How to structure your inventory
- Life quality improvements

![bg left](img/pexels-suzy-hazelwood-1226398.jpg)

---

# What is AVD

![bg right:40% fit](img/avd-roles-overview-dark.png)

<style scoped>section {font-size: 18px;}</style>

- [AVD is an open-source project](https://avd.arista.com)
- AVD stands for "Architect, Validate, and Deploy"
- [Supported designs](https://avd.arista.com/5.2/ansible_collections/arista/avd/roles/eos_designs/index.html) as of AVD 5.2:
  - L3LS EVPN / L2LS / MPLS / Campus
  - WAN AutoVPN/CV Pathfinder
- Roles:
  - `arista.avd.eos_designs` - abstracted to low level variables
  - `arista.avd.cli_config_gen` - parse templates to build plain text configs
  - `arista.avd.cv_deploy` - push configurations to Arista CloudVision Portal (replaced `eos_config_deploy_cvp`)
  - `arista.avd.eos_config_deploy_eapi` - push configs to switches directly. Based on [`arista.eos.eos_config`](https://docs.ansible.com/ansible/latest/collections/arista/eos/eos_config_module.html). [`arista.eos`](https://galaxy.ansible.com/arista/eos) is maintained by RedHat ⚠️
  - `arista.avd.eos_validate_state` - validate operational state of Arista EOS devices (with ANTA)

---

# Open Source: What to Expect

<style scoped>section {font-size: 16px;}</style>

![bg right w:600 opacity:85%](img/baby-crying-for-feature.png)

- Pros:
  - free to use
  - very agile and active community can outperform every alternative
  - open to any [feedback](https://github.com/aristanetworks/avd/discussions), [bug reports and feature requests](https://github.com/aristanetworks/avd/issues)
- Cons:
  - no direct revenue
    - no ads and sponsor buttons in AVD 🤑
    - core contibutors are working for Arista
  - best effort support by default
    - everything can be done - but it's not polite to ask when
    - customers have an option to <ins>buy Arista TAC support for AVD</ins> or Arista PS
- Help the community:
  - be part of the community - contribute ❗
  - understand the rules of the game
  - <ins>buy AVD support</ins> when you can or have higher requirements

---

# Tools and Prerequsites

<!-- ![bg opacity:30%](img/pexels-pixabay-33242.jpg) -->
![bg right fit](img/pexels-pixabay-33242.jpg)

- Check [ATD automation workshop](https://labguides.testdrive.arista.com/2024.3/automation/overview/)
- Git / GitHub
  - check [git-scm book](https://git-scm.com/book/en/v2)
  - [GitHub Git cheatsheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [VSCode](https://code.visualstudio.com/docs)
- Containers
  - [container.training](https://container.training/) can be a good start

---

# AVD Installation Options

<style scoped>section {font-size: 24px;}</style>

<div class="columns">
<div>

- Ansible CE (Community Edition)
  - free to use
  - check [AVD docs](https://avd.arista.com/) for the installation manual
- Ansible Automation Platform
  - paid RedHat support
  - check [AAP guide here](https://avd.arista.com/devel/docs/getting-started/avd-aap.html)
  - out of scope

</div>
<div>

After 4.9 (PyAVD is the foundation ⚠️)

```bash
# ansible-core will be installed as PyAVD requirement
pip install "pyavd[ansible]"
ansible-galaxy collection install arista.avd
# install community.general to support callback plugins, etc.
ansible-galaxy collection install community.general
```

- ⚠️ [PyAVD](https://pypi.org/project/pyavd/) is not intended to be used directly for customer projects
- If you have [an exception](https://github.com/arista-netdevops-community/CloudVisionPortal-Examples/tree/master/pyavd_examples) - this workshop is not for you. You know what to do 🤓 or have Arista PS support
- Ansible provides a lot of value, for ex. inventory management - use Ansible ⚠️

</div>
</div>

---

# How to Cook AVD Inventory

<style scoped>section {font-size: 22px;}</style>

![bg right](img/pexels-cottonbro-4253320.jpg)

- Be structured.
- Craft your environment and confirm that it works with some basic test.
- Create a minimalistic inventory and generate first configs.
- Grow your environment slowly, keep it clean and easy to read. Avoid workarounds.
- When required - step back to the last working setup
- Iterate
- Cloning existing repo and adjusting to the new setup is not always a good idea.
  - Make sure that you understand every single knob in the cloned inventory

---

# AVD Repository Building Blocks

<style scoped>section {font-size: 20px;}</style>

![bg right](img/pexels-isaiah-56332-833109.jpg)

- Environment
- ansible.cfg
- inventory
- variables
- playbooks
- life quality improvements
  - shortcuts
  - useful hacks

---

`Step 1`

# Environment

![bg left:40%](img/pexels-hissetmehurriyeti-47135946-7560118.jpg)

---

# Keep Your AVD Environment Clean

<style scoped>section {font-size: 24px;}</style>

<!-- Do not add page number on this slide -->
<!--
class: default
-->

![bg](img/pexels-goumbik-349609.jpg)

- ⛔ NEVER use "handcrafted" installation direclty on your machine. [It will break](https://xkcd.com/1987/)! Troubleshooting that is wasted time. ⏱️ → 🗑️
- ⛔ NEVER use root account!
- Feasible options:
  - Dedicated and well maintained VM
  - Virtual environment
  - Containers
    - AVD container images are in preview: fully functional, but breaking changes can happen any time
    - No Arista TAC support possible for customers who ordered AVD support
  - AnsibleEEs
    - This one is for RedHat support 💵

---

# Virtual Environment vs Containers

<!-- Do not add page number on this slide -->
<!--
class: invert
-->

<style scoped>section {font-size: 22px;}</style>

<div class="columns">
<div>

venv/pyenv 📦

- Pro:
  - simple and lightweight
  - no special tools required
- Breaks **often**. Troubleshooting complexity: **average**
- How it breaks:
  - multiple Pythons
  - incorrect requirements installation
  - broken path, custom ansible.cfg, tweaks, etc.
  - `../../../<ansible-collection>` 🤦 🙈

</div>
<div>

Containers 🐳

- Pro:
  - stable, portable
  - high level of isolation
- Breaks **rarely**. Troubleshooting complexity: **high**
- How it breaks:
  - permission issues 👑
    - check [this document](https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user) to UID requirements
  - broken Docker installation or host OS
  - tools can be "too heavy" for some users 🔨

</div>
</div>

---

# AVD Lab Environment: Options

<style scoped>section {font-size: 24px;}</style>

![bg right:33%](img/demo-time.jpeg)

- Linux
  - Docker CE
  - [Install directly](https://avd.arista.com/5.2/docs/installation/collection-installation.html)

    ```bash
    pip install "pyavd[ansible]"
    ansible-galaxy collection install arista.avd
    ```

- Windows
  - WSL2 + Docker CE
- MacOS
  - Podman

⚠️ Use a well maintained and healthy machine!

---

# Install Docker CE on Linux

<style scoped>section {font-size: 24px;}</style>

<div class="columns">
<div>

> The install process was tested on Ubuntu LTS

1. Install Docker on the host. You can used one-liner script for that. [Check Docker documentation](https://docs.docker.com/engine/install/ubuntu/) for details.
2. Add your user to the `docker` group.
3. Logout and login again to apply the changes.
4. Check the Docker version and run hello-world container to test functionality.
5. You must be able to run docker commands without sudo if the installation was succesful.

</div>
<div>

```bash
# install Docker
sudo curl -fsSL https://get.docker.com | sh
# add user to the docker group
sudo usermod -aG docker ${USER}
# test docker
docker --version
docker run hello-world
```

![img](img/docker-logo-blue.png)

</div>
</div>

---

# Environment Troubleshooting Cheatsheet

<style scoped>section {font-size: 24px;}</style>

<div class="columns">
<div>

- PyAVD is critical in latest AVD versions

  ```bash
  pip freeze | grep pyavd
  ```

- You can encounter Ansible "world writable directory" error in CI, remote containers, etc. due to very relaxed permissions and [Ansible thinking it's not secure](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#avoiding-security-risks-with-ansible-cfg-in-the-current-directory). Fix:

  ```bash
  $ printenv | grep ANSIBLE
  ANSIBLE_CONFIG=ansible.cfg
  ```

- Confirm that your environment is not isolated

  ```bash
  curl --user <login>:<password> --data "show version"
  --insecure https://<switch-mgmt-ip>:443/command-api --verbose
  ```

</div>
<div>

```bash
# Ansible collection setup
ansible --version
# check collections versions and install location
ansible-galaxy collection list
which ansible
# requirements
pip3 freeze
python3.XX -m pip freeze # may not be the same
# and where to find them
pip --version
python3 -c "import ansible as _; print(_.__file__)"
# python
python3 --version
which python
which python3
which python3.XX
python3.11 -c "import sys;print(sys.path)"
# user
whoami
id -u
id -g
echo ${HOME}
# check the PATH
echo ${PATH}
```

</div>
</div>

---

# Q&A

![bg left](img/pexels-valeriia-miller-3020919.jpg)

- [Ansible AVD](https://avd.arista.com/)
- [This repository](https://github.com/ankudinov/avd-slow-cooking)

```diff
- One more slide!
+ No more slides.
```

```bash
git commit -m "The END!"
```

<!-- Add footer starting from this slide -->
<!--
footer: '![h:20](https://www.arista.com/assets/images/logo/Arista_Logo.png)'
-->
