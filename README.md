# Fitxa tècnica: Com crear un servidor Ubuntu Server amb VirtualBox

## Objectiu

Crear una màquina virtual amb VirtualBox i instal·lar-hi Ubuntu Server. L'objectiu és tenir un servidor funcionant dins de l'ordinador, sense haver de tocar el sistema operatiu real. És la primera vegada que ho faig, així que he anotat tots els passos tal com els he anat fent.

## Materials

- Un ordinador amb Windows.
- El programa VirtualBox.
- La imatge ISO d'Ubuntu Server (`ubuntu-26.04.1-live-server-amd64.iso`).
- Uns 50 GB lliures al disc.
- Connexió a Internet.

## Procediment
1. Obrir VirtualBox i clicar el botó **Nova**.

2. Posar un nom a la màquina (jo li he posat `server ubuntu practica`) i seleccionar on es guarda. A **ISO Image** indicar on està el fitxer d'Ubuntu Server descarregat.
![Nom de la màquina i la ISO](/img/Captura%20de%20pantalla%202026-10-05%20151632.png)

3. Desmarcar la casella **Proceed with Unattended Installation**. Si no es desmarca, el programa ho instal·la tot sol i no es pot anar triant les opcions.

4. A l'apartat **Specify virtual hardware**, posar `3108 MB` de memòria i `4` processadors.
![Memòria i processadors](/img/Captura%20de%20pantalla%202026-10-05%20151703.png)
5. A l'apartat **Specify virtual hard disk**, posar `50 GB` de mida i deixar la resta com està. Clicar **Finish**.
![Mida del disc](/img/Captura%20de%20pantalla%202026-10-05%20151735.png)
6. Seleccionar la màquina a la llista i clicar **Inicia**.
![Resum de la màquina creada](/img/Captura%20de%20pantalla%202026-10-05%20151755.png)
7. Surt una pantalla negra amb lletres (el GRUB). Deixar seleccionat **Try or Install Ubuntu Server** i prémer Enter.
![Pantalla d'arrencada](/img/Captura%20de%20pantalla%202026-10-05%20151822.png)
8. Triar l'idioma: **Español**. A partir d'aquí tot es fa amb les fletxes del teclat i Enter, el ratolí no serveix.
![Triar l'idioma](/img/Captura%20de%20pantalla%202026-10-05%20151926.png)
9. Deixar marcat **Ubuntu Server** i anar a **Hecho**.
![Tipus d'instal·lació](/img/Captura%20de%20pantalla%202026-10-05%20151952.png)
10. A la pantalla de xarxa, només comprovar que surt una adreça IP (a mi em va sortir `10.0.2.15`) i continuar.
![Configuració de la xarxa](/img/Captura%20de%20pantalla%202026-10-05%20152018.png)
11. Deixar l'adreça del servidor de descàrregues tal com ve i continuar.

12. A la pantalla del disc, deixar marcat **Use an entire disk** i continuar. Quan avisa que esborrarà el disc, acceptar: només esborra el disc virtual, no el de l'ordinador real.
![Configuració del disc](/img/Captura%20de%20pantalla%202026-10-05%20152056.png)
13. Omplir el nom, el nom del servidor, l'usuari i la contrasenya.

    > Jo he posat `usuari` com a nom d'usuari i també `usuari` com a contrasenya, perquè és fàcil de recordar. Cadascú pot posar el que vulgui, però és important no oblidar-la perquè després cal per entrar.
![Crear l'usuari](/img/Captura%20de%20pantalla%202026-10-05%20152117.png)
14. A la pantalla d'**Ubuntu Pro**, deixar **Skip for now** i continuar.

15. A la pantalla d'SSH, marcar **Instalar servidor OpenSSH**. Serveix per poder connectar-se al servidor des d'un altre ordinador.
![Configuració d'SSH](/img/Captura%20de%20pantalla%202026-10-05%20152222.png)
16. Esperar. Van sortint moltes línies de text per pantalla; és normal i triga una estona.
![Instal·lant el sistema](/img/Captura%20de%20pantalla%202026-10-05%20152241.png)
17. Quan surt **Installation complete!**, anar a **Reiniciar ahora**.
![Instal·lació acabada](/img/Captura%20de%20pantalla%202026-10-05%20152307.png)
18. Quan torna a arrencar, escriure l'usuari i la contrasenya. El text de la contrasenya no es veu mentre s'escriu, però s'està escrivint igualment.
![Sessió iniciada al servidor](/img/Captura%20de%20pantalla%202026-10-05%20152329.png)
19. Ja dins del servidor, actualitzar-lo amb aquesta ordre:

```bash
    sudo apt update && sudo apt upgrade -y
```
## Comprovacions
- [ ] La màquina arrenca i surt la pantalla per iniciar sessió.
- [ ] Es pot entrar amb l'usuari i la contrasenya creats.
- [ ] Hi ha Internet dins del servidor (`ping -c4 google.com`).
- [ ] Es veu l'adreça IP del servidor amb l'ordre `ip a`.
- [ ] En apagar i tornar a engegar la màquina, tot segueix igual.

## Incidències i solucions
| Incidència | Solució |
|---         |---      |
| La instal·lació comença sola i no deixa triar res | Havia deixat marcada la casella *Proceed with Unattended Installation*. Cal esborrar la màquina i tornar-la a crear amb la casella desmarcada. |
| El ratolí es queda enganxat dins de la finestra | Prémer la tecla **Ctrl de la dreta** per alliberar-lo. |
| En escriure la contrasenya no es veu res | És normal, Linux no mostra els caràcters. S'escriu igualment i es prem Enter. |
| En reiniciar torna a sortir l'instal·lador | Cal treure la ISO des del menú **Dispositius → Unitats òptiques → Treu el disc**. |
| La màquina va lenta | Tancar programes de l'ordinador real o baixar la memòria assignada. |

## Recursos
- [Pàgina oficial de VirtualBox](https://www.virtualbox.org/)
- [Descàrrega d'Ubuntu Server](https://ubuntu.com/download/server)
- [Documentació consultada](https://docs.github.com/)
