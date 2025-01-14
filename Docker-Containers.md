# Docker

Docker containers offer strong isolation from the host system. Each container runs in its own isolated environment, and they don't have direct access to the host's filesystem or network. Once you have a container image configured for ART, you can run it on any system that supports Docker, regardless of the underlying operating system.

Docker allows you to define the exact environment in which ART runs. For custom use cases, you can create Dockerfiles that specify the required dependencies and configurations. This ensures that the test environment is consistent across different runs, making it easier to reproduce and analyze test results.

They can be easily started and stopped. When you're done with a test run, you can stop and remove the container, ensuring no traces or artifacts are left on your system.

They are lightweight and share the host OS kernel, which results in efficient resource utilization. You can either run all the tests in the provided docker image or you can also run multiple containers on a single host without significant overhead, making it suitable for running multiple ART tests concurrently with clean environments for each tests.

# Use cases

- Users can quickly pull down the ART images and test the functionality of `atomic-red-team` and `invoke-atomicredteam`.
- This can also be used to run periodic tasks (Kubernetes Cron Jobs/ CI-CD) and download the execution logs for continuous detection and validation.

# Installation

Refer the docs [here](https://docs.docker.com/engine/install/) to install Docker engine for your OS

# Usage

```sh
docker run -it redcanary/invoke-atomicredteam:latest
```

# Building locally

If you have different use cases or you want to add any dependency to the existing image, you can edit the Dockerfile and run the following commands to build and test them locally.

```sh
git clone https://github.com/redcanaryco/invoke-atomicredteam.git
cd invoke-atomicredteam/docker
# edit Dockerfile
docker build -t invoke-atomicredteam:latest .
docker run -it invoke-atomicredteam:latest
```


Note: These features can be used after the [PR merge](https://github.com/redcanaryco/invoke-atomicredteam/pull/154). 