
````markdown
# 🔧 Troubleshooting – Jamulus Server & Client

Tento dokument shrnuje nejčastější problémy při instalaci a používání **Jamulus serveru a klienta**.

---

## 1. Kontejner se hned vypne (`docker ps` nic neukazuje)
- Zkontroluj logy:
  ```bash
  docker logs jamulus-server
````

* Pokud vidíš hlášku `Unknown option`, použil jsi parametr, který tato verze Jamulusu nezná.
  → Odeber ho nebo spusť `--help` pro seznam dostupných parametrů.

---

## 2. Nelze se připojit na server

* Ověř, že port **22124/udp** je namapovaný:

  ```bash
  docker ps
  ```

  očekávaný výstup obsahuje:

  ```
  0.0.0.0:22124->22124/udp
  ```
* Pokud tam není, kontejner spusť znovu s:

  ```bash
  docker run -d --name jamulus-server -p 22124:22124/udp grundic/jamulus --nogui --server --port 22124
  ```

---

## 3. Firewall blokuje port

Na VPS může být aktivní firewall. Otevři UDP port:

```bash
firewall-cmd --add-port=22124/udp --permanent
firewall-cmd --reload
```

Pokud firewall není aktivní → není třeba nic řešit.

---

## 4. Jak zjistím svou veřejnou IP adresu?

Použij:

```bash
curl -4 ifconfig.me
```

Pak se v klientovi připoj na:

```
<tvá_IP>:22124
```

---

## 5. Zvuk funguje lokálně, ale v Jamulusu není slyšet

* Ujisti se, že máš v **klientovi** vybrané správné audio zařízení:

  * **Input Device** → mikrofon nebo audio interface (např. Rode, Yamaha)
  * **Output Device** → sluchátka nebo reproduktory
* Pokud používáš **Flatpak** na Linuxu, povol mu přístup k zařízením:

  ```bash
  flatpak override --user --device=all net.jamulus.Jamulus
  ```

---

## 6. Latence (delay) je příliš vysoká

* Zkontroluj kvalitu připojení (ping na server by měl být < 30 ms pro pohodlné hraní).
* V klientovi sniž **buffer size** (128–192 frames).
* Používej **kabelové připojení** místo Wi-Fi.

---

## 7. Interní mikrofon funguje, externí (USB) ne

* Ověř dostupná zařízení:

  ```bash
  arecord -l
  ```
* Pokud mikrofon vidíš v systému, ale ne v Jamulusu → spusť klienta s konkrétním zařízením:

  ```bash
  jamulus --device hw:1,0
  ```

---

## 8. Kontejner se nespustí, protože už běží starý

Restartuj server:

```bash
docker stop jamulus-server 2>/dev/null
docker rm jamulus-server 2>/dev/null
docker run -d --name jamulus-server -p 22124:22124/udp grundic/jamulus --nogui --server --port 22124
```



