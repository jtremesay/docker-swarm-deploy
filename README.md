# docker-swarm-deploy

## Description
`docker-swarm-deploy` is a GitHub Action that allows you to deploy your application as a Docker stack to a remote Docker Swarm. It automates the process of logging into Docker Hub, configuring SSH access, creating a Docker context, and deploying your stack.

## Inputs
The action requires the following inputs:

| Input Name          | Description                                           | Required |
|---------------------|-------------------------------------------------------|----------|
| `docker_username`   | Docker Hub username for pushing and pulling images.   | Yes      |
| `docker_api_key`    | Docker Hub API key associated with the username.      | Yes      |
| `remote_host`       | Remote host to connect to.                            | Yes      |
| `remote_host_name`  | Docker host name to connect to.                       | Yes      |
| `server_user`       | Remote user for SSH connection.                       | Yes      |
| `server_port`       | Remote port for SSH connection.                       | Yes      |
| `ssh_private_key`   | SSH private key for authentication.                   | Yes      |
| `ssh_known_hosts`   | SSH known hosts (e.g., SSH-ed25519 key).              | Yes      |
| `service_name`      | Name of the service in the Docker Swarm.              | Yes      |

## Workflow Steps
The action performs the following steps:

1. **Login to Docker Hub**: Logs into Docker Hub using the provided credentials.
2. **Create Private Key**: Configures SSH access by creating a private key, setting up the SSH config, and adding known hosts.
3. **Create Docker Context**: Creates and switches to a Docker context for the remote host.
4. **Checkout Repository**: Checks out the repository to access the `compose.yml` file.
5. **Deploy to Server**: Deploys the Docker stack using the `docker stack deploy` command with the provided service name.

## Example Usage
Below is an example of how to use this action in a GitHub workflow:
