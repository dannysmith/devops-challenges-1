# DevOps Challenge 1 - Common Requirements

## Time to complete: 7 hours

## Module Objective

This module is to give you a chance to practice the tools and principles that you have learned in the previous modules and to apply it to some real world examples. We will have some challenges which require you to do individual work, and others that will require team work.

## Objective

As a team you will need to build the environment that will provide the continuous pipeline, as code, and the permanent servers.

The whole environment needs to be repeatable in the event that it has a failure.

You should be able to create your own projects and have them run through the pipeline to your own servers to complete the next 2 tasks.

Based on what you have learned during the training you will need to identify the servers you need and how they will be used. You should consider how your systems will be built, and where, and whether they will be permanently available, or built on demand.

Your infrastructure environment can be on AWS if available, or on your local machines using Vagrant/VirtualBox and Docker, and can be spread across the physical machines with the rest of your team.

You will need to have servers that can;

- Run a Maiadb/MySQL Database
- Provide pipeline activities
- Host Chef cookbooks
- Be a registry for Docker containers
- An SCM (local or GitHub)

All the above services are shared between your team.

## Applications

- Java

  - Supplied by trainer
  - This code comes from a company called Conygre

- PHP

  - <https://sourceforge.net/directory/os:linux/?q=php%20web%20applications>

All application come with the source code, so you should place the source code in your own project directory.

You should not be using the same database name as any other member of your team, for very good reasons, that we hope you know. If you don't you'll have to discover why.

## Must haves:

- All applications must be deployed to Docker containers

  - The containers must be complete images containing the application code and the software and configuration for it to run
  - The containers must be stored in your own Docker registry

- You must have deployable containers to make the application work
- Both applications will require a MySQL database

  - They will share this database server
  - The server will be on a VM of it's own
  - The Docker containers must be able to communicate with the VM

- Passwords for Test **must** be different to those of production

  - If the application requires them up front

- All provisioning is to be done with Chef
- All testing with Kitchen and ServerSpec
- Be able to remove the database using Jenkins for a complete fresh deployment

## Permanent features;

- Jenkins C.I./C.D. server

  - You must have an automated pipeline that takes us from Development to Production
  - Servers that you build must be tested to ensure they meet requirements, including remote testing to ensure no firewalls are preventing the application from working

- Database server for Production

  - The test database can be a Docker image

- Docker Registry
- Git repository

  - You can use GitHub for this or GitLab server Docker container
  - Must contain your code for the environments
  - Must contain the application code to deploy

## Testing

Testing must be applied to all servers that are built (pretty much everything in this environment). This is ServerSpec testing through kitchen, and should be part of the Jenkins C.I.

Jenkins should be able to build a VM or Docker container and run the tests and then destroy it after it has created an image if the tests pass.

## Understanding the deployment

Like in the real world you are required to work out the infrastructure requirements for the application from the guide, and determine what you need to do in the way of automated steps to deploy the application.

Re-use is an important part of speed of delivery, so you may decide to use existing images from Docker, or cookbooks from the supermarket. This is perfectly acceptable. Important:

- All images must be up to date with patching
- You must know where the relevant directories are to server your application
- You must create your own Docker image for final deployment, which will contain the application.

## Documentation

You should produce a diagram of your infrastructure that explains how it works.
