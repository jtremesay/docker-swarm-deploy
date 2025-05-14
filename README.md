# docker-swarm-deploy

## Description
`docker-swarm-deploy` est une GitHub Action qui vous permet de déployer votre application en tant que stack Docker sur un Docker Swarm distant. Elle automatise le processus de connexion à Docker Hub, de configuration de l'accès SSH, de création d'un contexte Docker et de déploiement de votre stack.

## Inputs
L'action nécessite les entrées suivantes :

| Input Name            | Description                                           | Required | Default        |
|-----------------------|-------------------------------------------------------|----------|----------------|
| `docker_compose_file` | Le chemin vers le fichier docker-compose à utiliser.  | No       | `compose.yml`  |
| `docker_username`     | Nom d'utilisateur Docker Hub pour pousser et tirer des images. | Yes      |                |
| `docker_password`      | Clé API Docker Hub associée au nom d'utilisateur.     | Yes      |                |
| `remote_host`         | Hôte distant à connecter.                             | Yes      |                |
| `remote_user`         | Utilisateur distant pour la connexion SSH.            | Yes      |                |
| `remote_port`         | Port distant pour la connexion SSH.                   | No       | `22`           |
| `ssh_private_key`     | Clé privée SSH pour l'authentification.               | Yes      |                |
| `stack_name`          | Nom de la stack dans le Docker Swarm.                 | Yes      |                |

## Workflow Steps
L'action effectue les étapes suivantes :

1. **Connexion à Docker Hub** : Se connecte à Docker Hub en utilisant les identifiants fournis.
2. **Création de la clé privée** : Configure l'accès SSH en créant une clé privée, en configurant le fichier SSH et en ajoutant les hôtes connus.
3. **Création du contexte Docker** : Crée et bascule vers un contexte Docker pour l'hôte distant.
4. **Checkout du dépôt** : Récupère le dépôt pour accéder au fichier `docker-compose`.
5. **Déploiement sur le serveur** : Déploie la stack Docker en utilisant la commande `docker stack deploy` avec le nom de stack fourni.

## Example Usage
Voici un exemple d'utilisation de cette action dans un workflow GitHub :

```yaml
name: Deploy to Docker Swarm

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Docker Swarm
        uses: ./ # Remplacez par le chemin ou le repo de l'action
        with:
          docker_compose_file: compose.yml
          docker_username: ${{ secrets.DOCKER_USERNAME }}
          docker_password: ${{ secrets.DOCKER_USERNAME }}
          remote_host: ${{ secrets.REMOTE_HOST }}
          remote_user: ${{ secrets.REMOTE_USER }}
          remote_port: 22
          ssh_private_key: ${{ secrets.SSH_PRIVATE_KEY }}
          stack_name: my-docker-stack
```

## Côté serveur

```shell
# Create user
sudo useradd -m -s /usr/sbin/nologin -G docker github-ci

# Prepare ssh
sudo -u github-ci -s
ssh-keygen
echo 'command="docker system dial-stdio"' $(cat $HOME/.ssh/id_ed25519.pub) >> $HOME/.ssh/authorized_keys
```