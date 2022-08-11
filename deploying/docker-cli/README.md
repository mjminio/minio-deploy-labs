# Deploying MinIO using the Docker CLI

This method leverage Docker to allow you to quickly get up and running with a dev/test MinIO configuration.

We specify dev/test usage here because this method is ephemeral and does not by itself allow for automation and orchestration easily for a long lived, production grade environment.

If you would like to leverage Docker to deliver a more robust MinIO deployment, please look at the [Docker-Compose](../docker-compose/README.md) instructions.

## Prerequisites

The MinIO lab server already has Docker installed. If you performing this on a system you provisioned on your own not using the provided MinIO provision scripts, make sure that you have [Docker installed](https://docs.docker.com/engine/install/).

## Run Standalone Ephemeral MinIO on Docker

Reference: https://docs.min.io/docs/minio-docker-quickstart-guide

MinIO needs a persistent volume to store configuration and application data. For testing purposes, you can launch MinIO by simply passing a directory (/data in the example below). This directory gets created in the container filesystem at the time of container start. But all the data is lost after container exits.

Run the following command to deploy an ephemeral MinIO deployment:

`docker run -itd --name minio-ephemeral -p 9000:9000 -p 9001:9001 -e "MINIO_ROOT_USER=AKIAIOSFODNN7EXAMPLE" -e "MINIO_ROOT_PASSWORD=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY" quay.io/minio/minio server /data --console-address ":9001"` {{ execute }}

To view the logs for this container:

`docker logs minio-ephemeral` {{ execute }}

To access the Minio Web UI:

Link: http://LABSERVERNAME:9000
User: `AKIAIOSFODNN7EXAMPLE`
Pass: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

You could upload something into this instance, but note that because this is ephemeral, when we stop this container, that data you uploaded will not persist anywhere.

Stop the ephemeral MinIO container before continuing.:

`docker rm -f minio-ephemeral` {{ execute }}

# Run Standalone Persistent Data MinIO on Docker

To create a MinIO container with persistent storage, you need to map local persistent directories from the host OS to virtual config. To do this, run the below commands:

`mkdir -p /mnt/data/minio-persistent` {{ execute }}

`docker run -itd --name minio-persistent -p 9000:9000 -p 9001:9001 -v /mnt/data/minio-persistent:/data -e "MINIO_ROOT_USER=AKIAIOSFODNN7EXAMPLE" -e "MINIO_ROOT_PASSWORD=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY" quay.io/minio/minio server /data --console-address ":9001"` {{ execute }}

To view the logs for this container:

`docker logs minio-persistent` {{ execute }}

To access the Minio Web UI:

Link: http://LABSERVERNAME:9000
User: `AKIAIOSFODNN7EXAMPLE`
Pass: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

Once you access this via a browser, create a bucket and upload some non-sensitive piece of content as a test.

- In the UI, click `Create Bucket +` 
- Name the `minio-test-data`
- Click the `Upload` button while in the bucket dir in the UI and upload some non-sensitive information.

To view the information on the VM host disk:

`ls -lah /mnt/data/minio-persistent/minio-test-data` {{ execute }}

You should see the information you uploaded listed as standard data in the directory/file format on the OS.

Let's remove the MinIO Persistent container and see what happens to the data.

`docker rm -f minio-persistent` {{ execute }}

Now, let's check on our files again.

`ls -lah /mnt/data/minio-persistent/minio-test-data` {{ execute }}

Note that the files are still there. They persist even though MinIO is no longer running.

What happens if we redeploy MinIO Persistent and recreate that bucket?

`docker run -itd --name minio-persistent -p 9000:9000 -p 9001:9001 -v /mnt/data/minio-persistent:/data -e "MINIO_ROOT_USER=AKIAIOSFODNN7EXAMPLE" -e "MINIO_ROOT_PASSWORD=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY" quay.io/minio/minio server /data --console-address ":9001"` {{ execute }}

To access the Minio Web UI:

Link: http://LABSERVERNAME:9000
User: `AKIAIOSFODNN7EXAMPLE`
Pass: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

Once you access this via a browser, you will notice that MinIO rediscovered the bucket and files in the path and presented them in the UI.

Now, let's cleanup the data from this lab:

`docker rm -f minio-persistent ` {{ execute }}

`rm -rf /mnt/data/minio-persistent/minio-test-data` {{ execute }}

## MinIO Docker CLI Lab Complete

This completes our MinIO Docker CLI lab. Please proceed to the next sections in this course via the [MinIO Deployment Labs Homepage](../../README.md)
