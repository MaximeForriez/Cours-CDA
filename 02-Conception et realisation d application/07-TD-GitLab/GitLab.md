# `GitLab`

## Présentation

Ce T.D. permet d'utiliser un autre outil de versionnage très utilisé, `GitLab`. Vous pouvez créer un compte en ligne (https://gitlab.com/users/sign_in), mais, pour le tester, il semble qu'utiliser `Docker` soit plus pertinent. Ainsi, ce T.D. vous permettra d'opérer une synthèse des deux T.D. précédents et de vérifier si vous avez bien compris le fonctionnement de deux outils indispensables dans le cadre d'une démarche DevOps.

Ce dépôt reprend un projet `GitHub` 

- https://docs.gitlab.com/ee/install/docker/installation.html

- https://github.com/sameersbn/docker-gitlab?tab=readme-ov-file#installation

Son installation n'est pas à jour.

## Installation de l'image GitLab avec Docker

L'image `Docker` de `GitLab` s'installe avec un `docker-compose.yml`.

docker-compose up -d

Step 1. Launch a postgresql container

docker run --name gitlab-postgresql -d --env 'DB_NAME=gitlabhq_production' --env 'DB_USER=gitlab' --env 'DB_PASS=password' --env 'DB_EXTENSION=pg_trgm,btree_gist' --volume /srv/docker/gitlab/postgresql:/var/lib/postgresql sameersbn/postgresql:14-20230628

Step 2. Launch a redis container

docker run --name gitlab-redis -d --volume /srv/docker/gitlab/redis:/data redis:6.2

Step 3. Launch the gitlab container

docker run --name gitlab -d --link gitlab-postgresql:postgresql --link gitlab-redis:redisio --publish 10022:22 --publish 10080:80 --env 'GITLAB_PORT=10080' --env 'GITLAB_SSH_PORT=10022' --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' --env 'GITLAB_SECRETS_SECRET_KEY_BASE=long-and-random-alpha-numeric-string' --env 'GITLAB_SECRETS_OTP_KEY_BASE=long-and-random-alpha-numeric-string' --env 'GITLAB_SECRETS_ENCRYPTED_SETTINGS_KEY_BASE=long-and-random-alpha-numeric-string' --volume /srv/docker/gitlab/gitlab:/home/git/data sameersbn/gitlab:17.3.1
