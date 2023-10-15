# Ansible AWX-Operator Community.General Collection Execution Environment

This is a repo to build an Ansible Execution Environment that can communicate with hosts using the `community.general` collection.

it will work with the all of the `roles` or commands associated with it.

**Requirements**

You will need to have the following installed on the host machine in order to use this repo

- Docker
- Ansible-Builder
- An account on a container registration service (quay.io or docker hub for example)

The guide goes on the assumption that you have a basic understanding of `AWX-Operator` as well as `Ansible` and `Docker`.

# Steps to build the image for hosting

The following files need to be in the same folder

`bindep.txt`
`execution-environment.yml`
`requirements.txt`

As I mainly built this to use only for managing `Proxmox`, the `requirements.txt` file only has what it needs for it to work with those kinds of hosts. 
If you want to enhance it for some of the many other roles within the `community.general` collection, just add the additional python packages you need to `requirements.txt`.

You will then need to run the `ansible-builder` script to compile the image. 

Things you will want to have ahead of time is information about your container registration and the names and tags you wish to use.

# Docker Hub Example

The following would be the process for uploading to the Docker Hub container registration repo

**Login to the repo**

You will need to login to your docker hub account so that you can push the image when complete. This will go on the example of `giuffrelab` as the account going forward. 

```
docker login
```

Enter your username and password.

**Building the Image**

run the following command to compile the image. the `-v 3` at the end is for the verbosity. If you do not care about this run it without it. Levels of verbosity are 1-3.

```
ansible-builder build --tag giuffrelab/awx-community-general-ee -v 3
```

**Upload the image to the container registry**

You should now have a new folder called `context` containing a `Dockerfile` and a `_build` folder. You can ignore these for now. 

you can verify your `image` or the `name` by running 

```
docker images
```

next you will need to upload the compiled image to Docker Hub. change `giuffrelab` and the image name you chose to whatever you used

```
docker push giuffrelab/awx-community-general-ee
```

this will create your repo on your docker hub account for use

**Cleanup**

At this point if you want to remove all of this from your system run the following

remove the images from Docker

```
docker system prune --all
```

you can then delete the repo folder

# Using the new Execution Environment in AWX-Operator

to make use of the new EE in AWX you need to do the following settings changes

**Add docker hub account**

Under `Resources` and `Credentials` 
- Select `Add`
- Fill out the `name` field
- Under `Credential Type` select `Container Registry`
- Under `Authentication URL` enter `docker.io`
- Fill in `Username`
- Fill in `Password`
- `Save`

**Add the new Execution Environment**

change `giuffrelab` and the image name you chose to whatever you used

Under `Administration` and `Execution Environments`
- Select `Add`
- Fill out the `name` field
- Under Pull select `Always Pull`
- Under image put `giuffrelab/awx-community-general-ee:latest`
- Under `Registry credential` select the one created above
- `Save`


