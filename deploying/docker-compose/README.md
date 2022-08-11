# LAB: Deploying MinIO using Docker-Compose

Docker Compose allows defining and running single host, multi-container Docker applications.

With Compose, you use a Compose file to configure MinIO services. Then, using a single command, you can create and launch all the Distributed MinIO instances from your configuration. Distributed MinIO instances will be deployed in multiple containers on the same host. This is a great way to set up development, testing, and staging environments, based on Distributed MinIO.

## Prerequisites

The MinIO lab server already has Docker installed. If you performing this on a system you provisioned on your own not using the provided MinIO provision scripts, make sure that you have [Docker](https://docs.docker.com/engine/install/) and [Docker-Compose](https://docs.docker.com/compose/install/) installed.

## Deploy MinIO in Single Disk mode with Docker-Compose

We have trimmed down the original [Docker-Compose and Nginx files](https://github.com/minio/minio/tree/master/docs/orchestration/docker-compose) for single "host" single "disk" configuration for this example.

View the files here:

`/home/[USER]/minio/deploy/docker-compose/single/docker-compose.yml` {{ open }}

`/home/[USER]/minio/deploy/docker-compose/single/nginx.conf` {{ open }}

## Deploy Distributed MinIO with Docker-Compose
Reference: https://docs.min.io/docs/deploy-minio-on-docker-compose

View the files here:

`/home/[USER]/minio/deploy/docker-compose/distributed/docker-compose.yml` {{ open }}

`/home/[USER]/minio/deploy/docker-compose/distributed/nginx.conf` {{ open }}

## Deploying MinIO using Docker-Compose Lab Complete

This completes our Deploying MinIO using Docker-Compose. Please proceed to the next sections in this course via the [MinIO Deployment Labs Homepage](../../README.md)
