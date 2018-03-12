# README

This is a deployment script to deploy the Care Connect Reference Implementation into the NHS Developer Network infrastructure.

---

## Contents

* Setup
* Local deployment
* Jenkins deployment
    
---

## Setup 

* docker-coompose.yml - this is the main deployment file to deploy all the RI components into docker containers.

* docker-coompose.override.yml - this contains a set of default overrides for local deployment for testing locally. This will be merged in to the docker-compose.yml on a standard docker-compose. It exposes the CCRI on port 8099 locally.

* docker-compose.cloud.yml - this contains the differing config for building the project on the jenkins box to deploy into the developer network environment. It uses a slightly different directory structure with volumes mapped to a local drive, and will not expose the database on an externally accessible.

## Environment Variables

You'll need to specify some environment variables to run this compose file (either by creating a .env file in the same directory you run it from, or using a set of export commands). You can see details [here](https://nhsconnect.github.io/CareConnectAPI/build_ri_install.html). The only additional parameter in addition to those shown on this page is MYSQL_ROOT_PASSWORD which is the root password for the MYSQL database.

---

## Local deployment

```bash
docker-compose pull
docker-compose up -d
```

docker-compose.override.yml gets merged automatically.

To remove everything on your local machine, you'll need to remove the images with:

```bash
docker-compose rm
```

Then delete all the data volumes (currently mounted into directories in /docker-data)
And finally, delete the virtual network:

```bash
docker network rm careconnectridevelopernetwork_ccri_net
```

---

## Jenkins deployment

```bash
docker-compose -f docker-compose.yml -f docker-compose.cloud.yml pull
docker-compose -f docker-compose.yml -f docker-compose.cloud.yml up -d
```

docker-compose.cloud.yml gets pulled into docker-compose.yml instead of the override file

---


