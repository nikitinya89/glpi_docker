### Установка OpenVpn
```
sudo apt update && sudo apt install -y openvpn
```
Затем скопировать конфиг в папку `/etc/openvpn/client/` с расширением *.conf*. Туда же положить файл с паролем, например *srv.pass*
Добавить в конфиг `askpass '/etc/openvpn/client/srv.pass'`
### Запустить VPN
```
sudo systemctl start openvpn-client@srv
```
где srv - имя конфига (например *srv.conf*)
### Установка Docker
#### Добавление Репозитория
```
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
```
#### Установка
```
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
#### Для запуска docker без sudo
```
sudo usermod -aG docker $USER
```
### Скопировать docker-compose
```
git clone https://github.com/nikitinya89/glpi_docker
mkdir -p docker/glpi
mv glpi_docker/docker-compose.yml docker/glpi/docker-compose.yml
mv glpi_docker/mariadb.env docker/glpi/mariadb.env
```
---
**В файле mariadb.env задать свои пароли**

---
### Запуск docker контейнеров
```
docker compose up -d
```
### FusionInventory плагин
```
sudo apt install -y bzip2
tar -xvf glpi_docker/fusioninventory-10.0.6+1.1.tar.bz2 -C glpi_docker
sudo mv glpi_docker/fusioninventory /var/www/html/glpi/plugins/
```
