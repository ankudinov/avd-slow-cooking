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
