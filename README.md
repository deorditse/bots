https://github.com/user-attachments/assets/908eaaf2-d66f-446e-8dd8-a3676da2bc89

https://www.youtube.com/watch?v=BmZ50gHTLXw&list=WL&index=5&ab_channel=%D0%9A%D0%BE%D0%BD%D1%81%D1%82%D0%B0%D0%BD%D1%82%D0%B8%D0%BD

Step 1: Installing Docker

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```

## Сборка контейнеров

### Пересобрать и перезапустить все сервисы:

sudo docker compose up -d --build

### перезапустить все контейнеры

sudo docker compose down
sudo docker compose up -d

### пересобрать только один, например n8n-alliance:

sudo docker compose up -d --build n8n-alliance

### stop container

sudo docker stop n8n-test
sudo docker rm n8n-test

### Логи

sudo docker logs n8n-alliance

## разрешения

sudo chown -R 1000:1000 ./bots/alliance_trucks/data
sudo chown -R 1000:1000 ./bots/test/data
sudo chmod -R u+rwX ./bots/alliance_trucks/data
sudo chmod -R u+rwX ./bots/test/data

## Step 3: Setting up SSL with Certbot

Certbot will obtain and install an SSL certificate from Let's Encrypt.

sudo docker compose run --rm certbot certonly --webroot --webroot-path /var/www/certbot/ --dry-run -d deorbot.ru


Follow the on-screen instructions to complete the SSL setup.
Once completed, n8n will be accessible securely over HTTPS at your-domain.com.

IMPORTANT: Make sure you follow the above steps in order. Step 5 will modify your /etc/nginx/sites-available/n8n.conf file to something like this:
![image](https://github.com/user-attachments/assets/344187ec-5bcf-4d97-ad35-21b6562182e5)

## How to update n8n:

You can follow the instructions here to update the version: https://docs.n8n.io/hosting/installation/docker/#updating

Important: Take a backup of ~/.n8n:/home/node/.n8n
To create a backup, you can copy ~/.n8n:/home/node/.n8n to your local or another directory on the same VM even before deleting the container. And then, after updating and spinning up a new container, if you see the data getting lost, you can replace ~/.n8n:/home/node/.n8n with the one you saved earlier.

Ensure that your n8n instance is using a persistent volume or a mapped directory for its data storage. This is crucial because the workflows, user accounts, and configurations are stored in the database file (typically database.sqlite), which should be located in a directory that remains intact even when the container is removed.
In your docker-compose.yml, you should have something like this:

```bash
volumes:
- ~/.n8n:/home/node/.n8n
```

This mapping ensures that the .n8n directory on your host machine is used for data storage, preserving your workflows and configurations across container updates.

When you stop and remove the n8n container, you are only deleting the container instance itself, not the data stored in the persistent volume. As long as the volume is correctly configured, your workflows and accounts should remain unaffected.

But to avoid any chance of data loss you should take a backup of ~/.n8n:/home/node/.n8n before removing the container.
