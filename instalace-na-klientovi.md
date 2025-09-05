
# Jamulus – funkční setup (Server + Windows klient + FedoraCLient)

 popis konfiguraci Jamulus, která **funguje v reálném čase**, vidí mikrofon a umožňuje spojení klient–server.<br>
 Pokud chcete info jak instalovat na serveru [ctete prosim README](https://github.com/PajaspaceNet/jamulus-docker-server/blob/main/README.md)<br>

## Co jsme měli k dispozici 
* 1 OS Fedora   user station A  Laptop
* 1 OS Windows11  user station B  Laptop
* 1 Server RedHat /Centos/

---

## 1️⃣ Jamulus klient – Fedora

### Instalace
```
sudo dnf install flatpak pavucontrol
flatpak install flathub io.jamulus.Jamulus
```
Oprávnění pro mikrofon (Flatpak sandbox)
```
flatpak override --reset io.jamulus.Jamulus
flatpak override --user --device=all --socket=pulseaudio io.jamulus.Jamulus
```

Spuštění klienta
•	Standardně:
```
flatpak run io.jamulus.Jamulus
```
•	Pro nižší latenci (PipeWire + JACK):
```
pw-jack flatpak run io.jamulus.Jamulus
```

Kontrola mikrofonu

```
1.	Otevři pavucontrol
2.	Záložka Záznam (Recording) → Jamulus by měl být vidět
3.	Vyber správný vstupní mikrofon (ne „Monitor of…“)
4.	Zkontroluj výstup v záložce Přehrávání (Playback)
```
________________________________________
2️⃣ Jamulus klient – Windows
Instalace
`
1.	Stáhni z

```
 https://jamulus.io/software
```
3.	Spusť instalaci
Nastavení zvuku
```
•	Audio Device: WDM / DirectSound / WASAPI (bez ASIO lze spustit, ale ASIO4ALL je doporučeno pro nižší latenci)
•	Sample Rate: 48000 Hz
•	Buffer Size: 128 (nebo 64 pro nižší latenci)
•	Input Monitoring: zapnuto pro kontrolu vybuzení
```
Připojení k serveru
```
•	Connect → Add Server → IP adresu serveru a port 22124
•	Alternativně veřejný seznam, pokud je server registrován
```
________________________________________
3️⃣ Jamulus server – Fedora / kontejner
Spuštění

•	Headless, bez GUI:
```
Jamulus -s --nogui
```
•	Pro veřejnou registraci do seznamu:
```
Jamulus -s --nogui -e jamulus.fischvolk.de:22124
```
Síť
```
•	UDP port 22124 musí být otevřený / forwardovaný
sudo firewall-cmd --add-port=22124/udp --permanent
sudo firewall-cmd --reload
```
Docker / Podman varianta
```
•	Exponuj port, mountuj logy, nebo použij host network:
```
```
docker run -d --name jamulus-server \
  -p 22124:22124/udp \
  grundic/jamulus \
  --nogui --server --port 22124

  ```
________________________________________
4️⃣ Funkční nastavení a doporučení
```
•	Buffer / Sample Rate – nastavte pro nízkou latenci, ideálně Buffer 64–128, 48 kHz
•	Kontrola mikrofonu – vždy v pavucontrol (Linux) nebo Input Monitoring (Windows)
•	Firewall / Port forwarding – UDP 22124 otevřený
•	Audio driver Windows – bez ASIO lze spustit, doporučeno ASIO4ALL pro nejlepší výkon
•	PipeWire / JACK – na Linuxu pro nízkou latenci použít pw-jack
```
________________________________________
5️⃣ Shrnutí<br>
•	Tento setup funguje v reálném čase <br>
•	Mikrofon se vidí a vybuzuje <br>
•	Klient i server jsou schopni se připojit přes lokální síť i veřejně <br>
•	Všechny parametry a oprávnění jsou nastaveny tak, aby Jamulus běžel stabilně <br>

---
