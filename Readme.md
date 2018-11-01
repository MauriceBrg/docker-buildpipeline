# About 

This is a **test** project. I'm trying to build some containers that contain everything you need in order to automatically build docker containers from source.

# Components
- *GitLab* as a Git Repository (TODO)
- *Jenkins* as the BuildServer
- *Registry* as a private docker Registry (In Progress)

# Prerequisites
- Docker (I use 17.12.1-ce)
- openssl (for setting up the private registry)
- Common Sense
- Magic

# TODO-list
- Include Gitlab
- Document Setup
- Jenkins
    - Optimize Jenkinsfile of Jenkins
        - Option to build without cache
    - Make the image smaller (maybe install the docker client from the binaries)
    - The disable setup thing needs to be added to the JAVA_OPTS
- Docker-Compose
    - Make the Registry-Container's IP static (so you don't need to update your `/etc/hosts`)
- Registry
    - Find a way to automate setting up the ssl-certificate --> Possibly own docker image
    - Maybe install openssl in the registry, generate the certificate and echo the crt.
- Try to make the whole thing work with Kubernetes