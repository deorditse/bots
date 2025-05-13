Youtube Video Explanation: https://www.youtube.com/watch?v=Temh_Ddxp24
![Screenshot 2024-08-03 125607](https://github.com/user-attachments/assets/908eaaf2-d66f-446e-8dd8-a3676da2bc89)


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


Сборка контейнеров

🛠 Пересобрать и перезапустить все сервисы:
sudo docker compose up -d --build

перезапустить все контейнеры
sudo docker compose down
sudo docker compose up -d

пересобрать только один, например n8n-alliance:
sudo docker compose up -d --build n8n-alliance
