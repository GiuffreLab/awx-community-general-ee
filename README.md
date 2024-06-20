# Ansible AWX-Operator Community.General Collection Execution Environment

This is a repo to build an Ansible Execution Environment that can communicate with hosts using the `community.general` collection.

it will work with the all of the `roles` or commands associated with it.

This environment only has the dependencies for `Proxmox` nodes (`proxmoxer` and `results`) as noted in the `requirements.txt` file.

If you want to add other python packages, just add them to `requirements.txt` and build the image.

The compiled version of this is hosted on docker hub [Check Here](https://hub.docker.com/repository/docker/giuffrelab/awx-community-general-ee/general) and can be used directly in your AWX environment.

If you wish to learn how to build Execution Environments, [Check Here](https://github.com/GiuffreLab/building-execution-environments).

**Add the new Execution Environment**

change `giuffrelab` and the image name you chose to whatever you used

Under `Administration` and `Execution Environments`
- Select `Add`
- Fill out the `name` field
- Under Pull select `Always Pull`
- Under image put `giuffrelab/awx-community-general-ee:tagname`
- Under `Registry credential` select the one created above
- `Save`
