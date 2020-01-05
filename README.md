# handson-datadog

Le but de ce Handson est d'avoir une premiere approche sur Datadog.
Ce qui sera aborde:

- Monitoring (Systeme, Application)
- Logs
- Alerting

## Pre-requis

- Un compte AWS (WeScale ou perso) (Ca sera gratuit)
- Un compte Datadog

## Installation du serveur

L'AMI installe sera sous Debian 9.

- Suivre le tutoriel [Wordpress AWS](https://aws.amazon.com/getting-started/tutorials/launch-a-wordpress-website/)
- Se ssh en utilisant votre Key et l'user `bitnami`
- Activer la page de status nginx:
  - `sudo vi ~/stack/nginx/conf/bitnami/status.conf`
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
  - Editer `sudo vi ~/stack/nginx/conf/bitnami/status.conf`
  - Ajouter cette ligne a la fin
  - `include "/opt/bitnami/nginx/conf/bitnami/status.conf";`
- `sudo nginx -s reload`
- Tester: `curl localhost:81/nginx_status`

## Datadog Handson

- Installer l'agent Datadog sur le serveur (Votre choix, cela peut etre en allant sur la machine avec SSH, en utilisant Ansible etc...)
- Configurer l'agent pour avoir les infos de [Nginx](https://app.datadoghq.com/account/settings#integrations/nginx)
- Ajouter le forwarding des logs [Nginx](https://app.datadoghq.com/account/settings#integrations/nginx)
  - Les logs sont situe dans ce dossier: `/opt/bitnami/nginx/logs` 
- Ajouter l'integration [AWS](https://app.datadoghq.com/account/settings#integrations/amazon-web-services)
- Creer une alarme sur le nombre de 404 Nginx
- Creer un Dashboard (Amusez-vous :)
