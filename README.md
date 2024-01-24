# mgmt__grub
ansible role for grub automation
# requirements
# variables
grubby_task = "configure" or "remove" - used by grubby.yml to determine whether to add remove kernel arguments

grub_task = "configure" or "remove" - used by grub_settings.yml to determine whether to add remove grub variables
# dependencies
# examples
```
---

- name: usage example
  hosts: target_hosts

  tasks:

    - name: remove kernel arguments
      ansible.builtin.include_role:
        name: mgmt__grub
        tasks_from: grubby.yml
      vars:
        grubby_task: remove
        kernel_arguments:
          - crashkernel
          - quiet
          - rhgb

    - name: add kernel arguments
      ansible.builtin.include_role:
        name: mgmt__grub
        tasks_from: grubby.yml
      vars:
        kernel_arguments:
          - amd-iommu=on
          - iommut=pt
          - pci=nommconf

    - name: enable the grub menu - method 1
      ansible.builtin.include_role:
        name: mgmt__grub
        tasks_from: grub_settings.yml
      vars:
        grub_setting:
          - key: GRUB_HIDDEN_TIMEOUT
            value: 0
          - key: GRUB_TIMEOUT_STYLE
            value: menu
          - key: GRUB_TIMEOUT
            value: 5

    - name: enable the grub menu - method 2
      ansible.builtin.include_role:
        name: mgmt__grub
        tasks_from: grub_settings.yml
      vars:
        grub_task: remove
        grub_setting:
          - key: GRUB_HIDDEN_TIMEOUT
          - key: GRUB_TIMEOUT_STYLE

...
```
