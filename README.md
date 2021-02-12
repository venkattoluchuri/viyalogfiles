# SAS Viya Administration Resource Kit (Viya-ARK) - Log Cleanup Playbooks

## Introduction
The Log Cleanup repository contains a set of playbooks to list, move or delete files, and is configured by default to list, move or delete SAS Viya log archive files. It can be reconfigured to target other files.

## Requirements for Running the Viya Multi-Machine Services Utilities Playbooks
* The Log Cleanup playbooks must be placed under the sas_viya_playbook directory where SAS Viya was deployed. 
  The directory structure of this project must be preserved. 
  For example: ```sas_viya_playbook/viya-ark/playbooks/log-cleanup/```

## View or edit playbook configuration
View or edit log-cleanup-vars.yml in a text editor. This file defines important configuration settings, plus the path(s) and filename pattern(s) of files which are acted on by the log cleanup playbooks, and the path(s) to which they are moved by the log-cleanup-move.yml playbook. The configuration settings and the path_filepattern_moveto_list are explained in detail in that file.

## Running the Playbooks
To find log files (or whatever other files are targeted according to log-cleanup-vars.yml) and display them in Ansible output, execute:
```
ansible-playbook viya-ark/playbooks/log-cleanup/log-cleanup-find.yml
```

To list log files (or whatever other files are targeted according to log-cleanup-vars.yml) and write lists of files found on each SAS Viya host to a tempfile (defaults to /tmp/log-cleanup-list.txt) on the Ansible controller host, execute:
```
ansible-playbook viya-ark/playbooks/log-cleanup/log-cleanup-list.yml
```

To move log files (or whatever other files are targeted according to log-cleanup-vars.yml), execute the playbook below. By default ensure_moveto_directories_exist = no, and the playbook will fail to move files if the moveto directory does not exist on each host, resulting in playbook errors for the relevant task. When this happens, those files which could be moved are moved, and those files which could not be moved (because the moveto directory does not exist) are not moved. If you change ensure_moveto_directories_exist = yes in log-cleanup-vars.yml, then this playbook creates the 'moveto' directory if necessary if does not already exist on each host. The created directories are owned by root:root, with a directory access mode as defined in created_moveto_directory_mode in log-cleanup-vars.yml.
```
ansible-playbook viya-ark/playbooks/log-cleanup/log-cleanup-move.yml
```

To delete log files (or whatever other files are targeted according to log-cleanup-vars.yml), execute:
```
ansible-playbook viya-ark/playbooks/log-cleanup/log-cleanup-delete.yml
```
