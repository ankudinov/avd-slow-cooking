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
