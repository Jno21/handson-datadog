# handson-datadog

Le but de ce Handson est d'avoir une première approche sur Datadog.
Ce qui sera abordé:

- Monitoring (Systeme, Application)
- Logs
- Alerting

## Pre-requis

- Un compte AWS (WeScale ou perso) (Ca sera gratuit)
- Un compte Datadog

## Installation du serveur

L'AMI installe sera sous Ubuntu.

:warning: ATTENTION Veuillez installer le Wordpress avec Nginx SSL secure !!!!!!!! :warning:
- Suivre le tutoriel [Wordpress AWS](https://aws.amazon.com/getting-started/tutorials/launch-a-wordpress-website/)
- Se ssh en utilisant votre Key et l'user `bitnami`
- Activer la page de status nginx:
  - `sudo vi /opt/bitnami/nginx/conf/bitnami/status.conf`
  - Y mettre:
    ```
    server {
      listen 81;
      server_name localhost;

      access_log off;
      allow 127.0.0.1;
      deny all;

      location /nginx_status {
        # Choose your status module

        # freely available with open source NGINX
        stub_status;

        # for open source NGINX < version 1.7.5
        # stub_status on;

        # available only with NGINX Plus
        # status;
      }
    }
    ```
  - Sauver / Quitter
  - Editer `sudo vi /opt/bitnami/nginx/conf/bitnami/bitnami.conf`
  - Ajouter cette ligne a la fin
  - `include "/opt/bitnami/nginx/conf/bitnami/status.conf";`
- `sudo nginx -s reload`
- Tester: `curl localhost:81/nginx_status`

## Datadog Handson

- Installer l'agent Datadog sur le serveur (Votre choix, cela peut etre en allant sur la machine avec SSH, en utilisant Ansible etc...) [Ubuntu Installation](https://app.datadoghq.com/account/settings#agent/ubuntu)
- Configurer l'agent pour avoir les infos de [Nginx](https://app.datadoghq.com/account/settings#integrations/nginx)
- Ajouter le forwarding des logs [Nginx](https://app.datadoghq.com/account/settings#integrations/nginx)
  - Les logs sont situés dans ce dossier: `/opt/bitnami/nginx/logs`
- Creer une alarme sur le nombre de request/s Nginx
- Creer un Dashboard (Amusez-vous :)

## Bonus

- Ajouter l'integration [AWS](https://app.datadoghq.com/account/settings#integrations/amazon-web-services)
- Ajouter un / des [checks http](https://docs.datadoghq.com/integrations/http_check/) dans le client
- Ajouter le [monitoring de process](https://docs.datadoghq.com/infrastructure/process/?tab=linuxwindows)
- Creer une alerte sur le nombre de 4XX Nginx (Indice, il faut utiliser les logs)
- Faire une Lambda (Faite une lambda hello world) puis activé les metriques dans Datadog (Indice: AWS Integration)

### Aide

- [Information Agent Ubuntu](https://docs.datadoghq.com/agent/basic_agent_usage/ubuntu/?tab=agentv6v7)
