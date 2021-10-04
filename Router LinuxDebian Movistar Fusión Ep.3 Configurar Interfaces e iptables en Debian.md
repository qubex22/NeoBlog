#### En este capítulo haremos la configuración de las interfaces del router para poder tener conexión y entenderemos como funciona iptables para tener un firewall básico.
<p>
<a href="https://neo.zer01.eu/2021/09/19/router-linux-debian-movistar-fusion-ep-2-esquema-de-red/">
<img border="0" alt="W3Schools" src="https://i.postimg.cc/5tdbj1xx/esquemared-drawio.png" width="260" height="240">
</a>
</p>

# CONFIGURAR INTERFACES
En este caso la interfaz que recibe las 3 VLAN es la enp1s0, pero en vuestro caso puede ser enp1sX o ethX, donde X es el numero de la interfaz, depende de la version de debian o ubuntu
## INTERNET
- Empezamos con la interfaz Wan de internet que corresponde a la vlan 6. Para indicar la vlan en la interfaz ponemos **nombredelainterfaz.x** donde x es la vlan
```bash
# wan
auto enp1s0.6
iface enp1s0.6 inet manual
description wan
```

## TELEVISION
- La tv corresponde con la vlan 2. Además, debemos poner la ip que hemos apuntado en el [primer capitulo](https://neo.zer01.eu/2021/09/19/router-linux-movistar-fusion-ep-1-idont-ip-imagenio-ont-propia/ "primer capitulo") en address, la máscara en netmask y la puerta de enlace en gateway
```bash
# tv
auto enp1s0.2
iface enp1s0.2 inet static
description tv
address 10.xx.xx.xx
netmask 255.192.0.0
gateway 10.64.0.1
```

## VOZ
- Este apartado lo voy a dejar para otro capítulo, por eso está comentado, pero podéis ir configurando la interfaz para ahorrar trabajo en el futuro, simplemente es poner la interfaz en DHCP
```bash
# voz
#auto enp1s0.3
#iface enp1s0.3 inet dhcp
#description voz
```

## INTERFAZ LAN
- Configuramos la interfaz la en otra boca del router, le ponemos una ip privada fija y la mascara de subred deseada (en mi caso /23, pero normalmente con /24 es suficiente). Además ponemos una regla básica de queuing.
```bash
#LAN
auto enp2s0
iface enp2s0 inet static
description lan
address 172.16.1.254
netmask 255.255.254.0
# this does some basic fair queuing
up tc qdisc add dev enp2s0 root sfq perturb 10
```

## INTERFAZ LAN TV
- Como mi router tiene 4 bocas, en este caso he decidido separa la lan de la tv con la de internet, con diferentes redes privadas, pero podría estar perfectamente en la misma LAN
```bash
auto enp3s0
iface enp3s0 inet static
description lantv
address 192.168.98.1
netmask 255.255.255.0
```

## INTERFAZ LAN VOZ
- Igual que con la LAN TV, hacemos otra red para la lan de voz
```bash
auto enp4s0
iface enp4s0 inet static
description voz
address 192.168.99.1
netmask 255.255.255.0
```

## PPPoE
- Esta parte es muy importante, aquí declaramos la interfaz ppp, e indicamos qué interfaces tienen que estar up para realizar la conexión. Por último con la línea `provider dsl-provider` autenticamos la conexión ppp con el usuario y contraseña que luego configuraremos
```bash
auto wan
iface wan inet ppp
pre-up /bin/ip link set enp1s0 up
pre-up /bin/ip link set enp1s0.6 up # line maintained by pppoeconf
provider dsl-provider
pre-up sleep 10
```

# CONEXION PPPOE
Tenemos que instalar el paquete `pppoe`
```bash
sudo apt install pppoe
```


Creamos y editamos el archivo `dsl-provider`
```bash 
nano /etc/ppp/peers/dsl-provider 
```
Copiamos y pegamos en el archivo dsl-provider. **OJO** cambiamos enp1s0.6 por la interfaz que cada uno ha configurado para wan internet. El usuario es el mismo para todos los clientes de movistar

```bash
ifname wan
noipdefault
usepeerdns
defaultroute
replacedefaultroute
hide-password
lcp-echo-interval 15
lcp-echo-failure 3
connect /bin/true
noauth
persist
mtu 1492
noaccomp
default-asyncmap
plugin rp-pppoe.so enp1s0.6
logfile /var/log/ppp.log
user "adslppp@telefonicanetpa"
```

Editamos el archivo pap-secrets
```bash
nano /etc/ppp/pap-secrets
```
Pegamos esta línea que contiene el usuario y contraseña de movistar, que es el mismo para todos. ` "adslppp@telefonicanetpa" * "adslppp" `

# ACTIVAR ENRUTAMIENTO
Para ello tenemos que editar este archivo `/etc/sysctl.conf` con estas 2 líneas
```bash
net.ipv4.ip_forward=1
net.ipv6.conf.all.forwarding=1
```
---
Finalmente reiniciamos, conectamos todo físicamente y deberíamos ver una ip pública en la interfaz wan
```bash
ip address

wan: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1492 qdisc pfifo_fast state UNKNOWN group default qlen 3
    link/ppp
    inet 83.xx.xx.xx peer 192.168.144.1/32 scope global wan
       valid_lft forever preferred_lft forever

```
---
En los próximos capítulos configuraremos iptables y dhcp












