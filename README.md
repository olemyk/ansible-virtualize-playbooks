# Ansible Playbooks for  IBM Spectrum Virtualize


Ansible Playbooks examples for the Ansible Collection - [ibm.spectrum_virtualize](https://github.com/ansible-collections/ibm.spectrum_virtualize/blob/master/README.md)


### Before you can use the playbooks:
 
1: Install Ansible on you computer. 

2: You need to install the IBM Spectrum Virtualize collection hosted in Galaxy:
  
  - To install the IBM Spectrum Virtualize collection hosted in Galaxy:
    
    
    ansible-galaxy collection install ibm.spectrum_virtualize
    
    
   - To upgrade to the latest version of the IBM Spectrum Virtualize collection:
    
    ansible-galaxy collection install ibm.spectrum_virtualize --force
  
3: Download Playbook repository

   - Simply download (clone) the repository.
   
   ```bash
   clone https://github.com/olemyk/ansible-virtualize-playbooks ansible-virtualize-playbooks/
   ```

4: *Check and run the playbooks in the examples folder*

   - Update the values in /vars/specv_vars.yml if needed.
   
   - Run Playbooks with:
        - `ansible-playbook specv_ansible_playbook_*.yml`
   - For verbose output you can run the ansible-playbook command with the option: -v
        - `ansible-playbook specv_ansible_playbook_*.yml -v`
   - To use Ansible extra-vars function. 
        - You can use the playbook with the vars defined. 
                    
            `ansible-playbook /examples/specv_ansible_playbook_simple_host_volume_map_extra_vars.yml --extra-vars "hostname=ansible-host1 fcwwpn=10000000C9609C78:10000000C9609C77 state=present vdiskname=ansible-vdisk1 vdisksize=10 vdiskunit=kb mdiskgrp=FS840-2"  -vv `



## Requirements

- Ansible version 2.9 or higher
- [ibm.spectrum_virtualize](https://github.com/ansible-collections/ibm.spectrum_virtualize/blob/master/README.md)



### Options for Ansible Virtualize Modules.


### Spectrum Virtualize Modules

- **ibm_svc_info - Collects information on the IBM Spectrum Virtualize system**
- **ibm_svc_host - Host management for IBM Spectrum Virtualize**
- **ibm_svc_mdisk - MDisk managment for IBM Spectrum Virtualize**
- **ibm_svc_mdiskgrp - Pool management for IBM Spectrum Virtualize**
- **ibm_svc_vdisk - Volume management for IBM Spectrum Virtualize**


- PS:  Missing option info about mdisk and mdiskgrp, will be add later
    
    
### For all modules - Common option

 | **Option**    | **Type**    | **Required**  | **Default**| **Description** |
| :------------ |:---------------:| ----------:| ------:|--------------------------------:|
| clustername   |     str         |   true     |        | The hostname or management IP of the SV storage system |  
| domain        |     str         |   false    |        | Domain for the IBM Spectrum Virtualize storage system
| username      |     str         |   true     |        | REST API username for the IBM Spectrum Virtualize storage system
| password      |     str         |   true     |        | REST API password for the IBM Spectrum Virtualize storage system
|validate_certs | bool (true/false)|  false    |  false | Validate certification
| log_path      | path            |   false    |        | Debugs log for this file





### This module manages hosts on IBM Spectrum Virtualize.
#### Module: ibm_svc_host

| **Option** | **Type**| **Required**  | **Default**|**Choices** | **Description** | 
| :---------- |:---------| --------:| -------------:|-----------:|----------------:|
| name        |      str |  true    |               |            | Specifies a name or label for the new host object |  
| fcwwpn      |      str |  false   |               |            | List of Initiator WWN to be added to the host
| iscsiname   |      str |  false   |               |            | Initiator IQN to be added to the host system
| iogrp       |      str |  false   | 1:2:3:4       |            | Specifies a set of one or more (I/O) groups that the host can access the volumes from
| protocol    |      str |  false   | scsi          |"scsi", "nvme" | Specifies the protocol used by the host to communicate with the storage 
| type        |      str |  false   | generic       |            |Specifies the type of host
| state       |      str |  false   | generic       |  absent, present |Creates (C(present)) or removes (C(absent)) a host

### This module manages volumes on IBM Spectrum Virtualize.
#### Module: ibm_svc_vdisk
| **Option** | **Type**| **Required**  | **Default**|**Choices** |  **Description** | 
| :---------- |:---------| --------:| -------:|-------------------------:|------:|
| name        |      str |  true    |         |                          | Specifies a name to assign to the new volume |  
| mdiskgrp    |      str |  true    |         |                          |Specifies one or more storage pools name to use when you are creating this volume
| easytier    |          |  false   |   off   | on, off, auto            | Defines use of easytier with VDisk
| size        |          |  false   | 1:2:3:4 |                          | Defines size of VDisk 
|protocol     |      str |  false   | scsi    | b, kb, mb, gb, tb, pb    |"Defines the size optoin for the storage unit 
| state       |      str |  false   | generic |  absent, present         |Creates (C(present)) or removes (C(absent)) a volume



## This module gathers various information from the IBM Spectrum Virtualize.
### Module: ibm_svc_info

| **Option** | **Type**| **Required** | **Description** | 
| :---------- |:---------| ----------:|------------------:|
| gather_subset| list    |  false     |  Collects storage information


#### gather_subset:

|**Values**   | **Description**                       | 
| :---------- |:------------------------------------| 
|all          | list of all IBM Spectrum Virtualize entities supported by the module |
|vol  		  | lists information for VDisks
|pool  	      | lists information for mdiskgrps
|node         | lists information for nodes
|iog          | lists information for I/O groups
|host         | lists information for hosts
|hc           | lists information for host clusters
|fc           | lists information for FC connectivity
|fcport       | lists information for FC ports
|targetportfc | lists information for WWPN which is required to set up FC zoning and to display the current failover status of host I/O ports
|fcmap        | lists information for FC maps
|fcconsistgrp | displays a concise list or a detailed view of FlashCopy consistency groups
|iscsiport    | lists information for iSCSI ports
|vdiskcopy    | lists information for volume copy
|nf           | lists information for NVMe fabric
|array        | lists information for array MDisks
|system       |displays the storage system information
|default      | "all"



- Test for Yml


```yaml
- name: Using the IBM Spectrum Virtualize collection
  collections:
    - ibm.spectrum_virtualize
  gather_facts: no
  connection: local
  hosts: localhost
  tasks:
    - name: Gather info from storage
      ibm_svc_info:
        clustername: specvip
        domain:
        username: superuser
        password: yourpassword
        log_path: logs/playbook.debug
        name: gather_info
        state: info
        gather_subset: targetportfc
```
 