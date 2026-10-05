# Fitxa tècnica: Creació d'un servidor Ubuntu Server en una màquina virtual amb VirtualBox

## Objectiu

Crear una màquina virtual amb Oracle VirtualBox i instal·lar-hi Ubuntu Server 26.04.1 LTS, deixant el servidor operatiu, amb accés remot per SSH i connexió a Internet. En acabar, qualsevol company hauria de poder repetir el procediment des de zero només seguint aquest document.

## Materials

- Equip amfitrió amb Windows 11 i permisos d'administrador.
- Oracle VirtualBox 7 o superior.
- Imatge ISO `ubuntu-26.04.1-live-server-amd64.iso` (uns 2,73 GB).
- Virtualització activada a la BIOS/UEFI (VT-x per a Intel, AMD-V per a AMD).
- Espai lliure al disc de l'amfitrió: mínim 50 GB.
- Connexió a Internet per descarregar paquets i actualitzacions.

## Procediment

## Comprovacions
- [ ] La màquina virtual arrenca des del disc dur i no des de la ISO.
- [ ] Es pot iniciar sessió amb l'usuari creat.
- [ ] El sistema mostra Ubuntu 26.04.1 LTS (`lsb_release -a`).
- [ ] La interfície `enp0s3` té l'adreça `10.0.2.15` (`ip a`).
- [ ] Hi ha connexió a Internet (`ping -c4 google.com`).
- [ ] El servei SSH està actiu (`systemctl status ssh`).
- [ ] El disc està configurat amb LVM (`lsblk`).

## Incidències i solucions
| Incidència | Solució |
|---         |---      |
| La instal·lació s'inicia sola i no deixa triar opcions | S'havia deixat marcada *Proceed with Unattended Installation*; esborrar la màquina i tornar-la a crear amb l'opció desmarcada. |
| Només apareixen opcions de 32 bits | Activar la virtualització (VT-x / AMD-V) a la BIOS i desactivar Hyper-V a Windows. |
| En reiniciar torna a arrencar l'instal·lador | Desmuntar la ISO des de Dispositius → Unitats òptiques → Treu el disc. |
| No es pot connectar per SSH des de l'amfitrió | En mode NAT cal redirigir ports, o canviar l'adaptador a mode pont. |
| El ratolí i el teclat es queden atrapats | Prémer la tecla amfitriona (Ctrl dreta) per alliberar-los. |

## Recursos
- [Manual oficial de VirtualBox](https://www.virtualbox.org/manual/)
- [Descàrrega d'Ubuntu Server](https://ubuntu.com/download/server)
- [Documentació consultada](https://docs.github.com/)

1. Obrir VirtualBox i prémer el botó **Nova**. Posar el nom de la màquina (`server ubuntu practica`), triar la carpeta on es desarà i seleccionar la ruta de la imatge ISO. VirtualBox detecta automàticament que és un sistema Linux/Ubuntu de 64 bits.

   > **Important:** cal desmarcar la casella *Proceed with Unattended Installation*. Si es deixa marcada, VirtualBox fa la instal·lació automàticament i no es poden triar les opcions.

2. A **Specify virtual hardware**, assignar `3108 MB` de memòria base i `4` processadors.

3. A **Specify virtual hard disk**, crear un disc nou de `50 GB`, de tipus VDI i amb reserva dinàmica.

4. Comprovar al gestor de VirtualBox que el resum és correcte: memòria, processadors, disc de 50 GB i adaptador de xarxa 1 en mode NAT.

5. Prémer **Inicia**. Al menú GRUB, seleccionar **Try or Install Ubuntu Server**.

6. Triar l'idioma de l'instal·lador (Español) i acceptar la distribució de teclat proposada.

7. A *Choose the type of installation*, deixar seleccionat **Ubuntu Server** i continuar amb **Hecho**.

8. A *Network configuration*, verificar que la interfície `enp0s3` ha rebut una adreça per DHCP (`10.0.2.15/24`, pròpia del mode NAT).

9. Deixar el mirall de descàrrega per defecte (`http://archive.ubuntu.com/ubuntu/`).

10. A *Guided storage configuration*, triar **Use an entire disk** i marcar **Set up this disk as an LVM group**.

11. A *Profile configuration*, omplir el nom, el nom del servidor, el nom d'usuari i la contrasenya.

    > **Recomanació:** en aquesta pràctica s'ha fet servir `usuari` com a nom d'usuari i també com a contrasenya, perquè sigui fàcil de recordar. Cadascú pot posar-hi el que vulgui; en un servidor real caldria una contrasenya robusta.

12. A la pantalla d'**Ubuntu Pro**, deixar marcat **Skip for now**.

13. A *SSH configuration*, marcar **Instalar servidor OpenSSH** i **Permitir autenticación con contraseña por SSH**.

14. Esperar que acabi la instal·lació del sistema.


15. Quan aparegui **Installation complete!**, seleccionar **Reiniciar ahora** i treure la ISO de la unitat òptica.

16. Iniciar sessió amb l'usuari i la contrasenya creats.

17. Actualitzar el sistema:

```bash
    sudo apt update && sudo apt upgrade -y
```