
## Vitam Vas na projektu Jamulus-docker-server.  

**Proc vzniknul tento program** - z potreby hrat online , aby mel clovek na druhe strane slysel v realne case a ne se zpozdenim<br>
Receno technicky **Jamulus je open-source software** -  pro hraní hudby online s nízkou latencí.<br>
**Proc docker** - protoze instalacni balicky jsou psane pro debian , no a ja mam CentOS na serveru  , takze nejjednodussi bylo<br>
pouzit docker , a pustit to v nem <br>
Repo obsahuje **docker-compose** kdyby nekdo chtel to spouste prez nez nej /nekolik imigu atd/, nebo **primou moznost start scriptem**.<br>

## jak to  funguje 

### Zhruba takto <br>
Klienti posílají audio přes UDP.<br>
Server provede Opus dekompresi → jitter buffer → mix.<br>
Výsledek se opět komprimuje (Opus) a posílá všem zpět.<br>



```
   [ Client A ]      [ Client B ]      [ Client C ]
        |                 |                 |
        v                 v                 v
   +---------------------------------------------+
   |                  Server                     |
   |                                             |
   |  [UDP recv] -> [Opus decode] -> [JitterBuf] |
   |            \       |       /                |
   |             \      |      /                 |
   |              +---> [Mixer]                   |
   |                      |                       |
   |          [Opus encode + UDP send]            |
   +---------------------------------------------+
        |                 |                 |
        v                 v                 v
   [ Mix to A ]     [ Mix to B ]     [ Mix to C ]
```

[Podrobneji zde](https://jamulus.io/PerformingBandRehearsalsontheInternetWithJamulus.pdf?utm_source=chatgpt.com)

## Doporuceny postup 


* Readme .... seznameni se s serverm principy a dale pokud chcete instalovat na serveru  <br>
* instalace-na-klientovi Linux/Win ... [jak nainstalujete na jednotlivych klientech](https://github.com/PajaspaceNet/jamulus-docker-server/blob/main/instalace-na-klientovi.md)<br> 
*  troubleshooting ... [troubleshouting](https://github.com/PajaspaceNet/jamulus-docker-server/blob/main/troubleshooting.md)<br>
* screenshots ... [screenshoty s nastavenim klienta na windows](https://github.com/PajaspaceNet/jamulus-docker-server/blob/main/screenshots.md)

```
jamulus-docker-server/
├── README.md
├── docker-compose.yml
├── .gitignore
├── instalace-na-klientovi.md
├── troubleshooting.md
├── screenshots.md
```





### 📄  Docker instalace

```markdown
Jamulus Server (Docker running)
```
**Konfigurace a  spuštění serveru**
Jadro jamulus , instrukce, KB  atd ... se nachazi take zde  - [Jamulus](https://jamulus.io) 

**Jamulus je open-source software** -  pro hraní hudby online s nízkou latencí.



## 🔧 Předpoklady

Na serveru musí být nainstalovaný Docker a Docker Compose - *pokud chcete spoustet s compose :-).

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

**nebo bez Compose primo** 

```bash
docker run -d --name jamulus-server \
  -p 22124:22124/udp \
  grundic/jamulus \
  --nogui --server --port 22124
```

To spustí Jamulus server na portu `22124/udp`.


## Zastaveni stareho a spusteni noveho klienta 

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

Vice na *instalace-na-klientovi Linux/Win*... [jak nainstalujete na jednotlivych klientech](https://github.com/PajaspaceNet/jamulus-docker-server/blob/main/instalace-na-klientovi.md)<br> 





