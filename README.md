# jamulus-docker-server
```
jamulus-docker-server/
├── README.md
├── docker-compose.yml
├── .gitignore

```




### 📄  Docker instalace

````markdown
# Jamulus Server (Docker)

Tento repozitář obsahuje jednoduchou konfiguraci pro spuštění
[Jamulus](https://jamulus.io) serveru v Dockeru.

Jamulus je open-source software pro hraní hudby online s nízkou latencí.

---

## 🔧 Předpoklady

Na serveru musí být nainstalovaný Docker a Docker Compose.

### Instalace Dockeru (Linux, CentOS)

```
curl -fsSL https://get.docker.com | sh
sudo systemctl enable docker
sudo systemctl start docker
````

Přidání uživatele do skupiny `docker` (aby nebylo potřeba `sudo`):

```bash
sudo usermod -aG docker $USER
# odhlásit a znovu přihlásit
```

### Instalace Docker Compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

---

## 🚀 Spuštění Jamulus serveru

Stačí spustit:

```bash
docker-compose up -d
```

To spustí Jamulus server na portu `22124/udp`.


## Zastaveni stareho a stareho a spusteni noveho klienta 

```
docker stop jamulus-server 2>/dev/null; docker rm jamulus-server 2>/dev/null; \
docker run -d --name jamulus-server \
  -p 22124:22124/udp \
  grundic/jamulus \
  --nogui --server --port 22124
```



## ⚙️ Připojení klienta

Z klienta (Jamulus GUI na Windows / Linux / macOS) se připojíte na:

```
<IP_adresa_VPS>:22124
```





