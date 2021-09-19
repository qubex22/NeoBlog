\#### Para tener un poco más claro el objetivo os explico el esquema de red que he diseñado para mi caso particular.

\------------

![](<https://i.postimg.cc/5tdbj1xx/esquemared-drawio.png>)

\------------

\- En primer lugar la ONT, como ya comenté en la anterior entrada, es el modelo `HG8310M de Huawei`, **IMPORTANTE**, tiene que tener el conector verde **SC APC**, el azul no es compatible con nuestra instalación de movistar. También he probado con una **Alcatel g-010g-p** que tenía por casa, es la que suele poner vodafone, y funciona perfectamente. Avisaros de que si tenéis la **ONT de ubiquiti** (ufiber nano/loco), la TV no os va a funcionar, ya que no permite multicast. [Aquí](<https://community.ui.com/questions/Adding-IGMP-multicast-on-UFiber-Nano-G/c0e37bc7-e14b-4a79-8ad1-90c6e168d0c7> "Aquí") os dejo un enlace de su foro si teneis curiosidad

\- Movistar hace uso de **VLAN** en su infraestructura, esto les permite mandar por el mismo canal varios servicios (TV, internet, Voz ip). Sin embargo cada uno de estos servicios utiliza diferentes sistemas:

	1. Los datos de **internet** viajan a traves del protocolo **PPPoE**. Un protocolo de encapsulación bastante maduro que, aunque no es muy eficaz a altas velocidades y luego veremos por qué, su configuración es sencilla para nosotros y la implementacion de la infraestructura poco costosa para Movistar.

	2. En cuanto al servicio de **TV**, Movistar nos asigna una direccion **ip fija**, la cual, se telecarga al router que nos instalan en el primer encendido. Por ello que sea necesario apuntarla, como expliqué en la entrada anterior. Además Movistar utiliza el protocolo **RIP de rutas dinámicas**, con el cual nos mandará las rutas necesarias para la visualizacion de la TV.

	3. Por último movistar asigna ip dinámicas por **DHCP** para el servicio de Telefonía **Voz IP**

\- En mi caso como **router** he utilizado un **mini PC** fanless, también de mano de Aliexpress, y le he instalado debian. Sus especificaciones

		CPU: J3160

		RAM: 4 GB

		SSD: 64 GB

	Decir que la CPU se queda algo escasa para conseguir velocidades de 1gbps por culpa del PPPoE, ya que este procotolo solo utiliza 1 hilo/nucleo de la CPU. A mi se me queda en 850mbps +/-. Si tenéis la tarifa de 1 gpbs y queréis full esa velocidad os recomiendo un i3 o un J4125 que son más potentes en mononúcleo. Si queréis un router más profesional os recomiendo el `EdgeRouter ER4` de ubiquiti, no es tan potente como un mini PC pero está mucho mejor optimizado y llega a velocidades de gbps sin problemas.

\- Creo que un **switch PoE** es esencial si quieres poner un punto de acceso. En mi caso tengo un `NetGear GS108PE`, es gestionado, pero no sería necesario si no vas a tener varias vlan en casa (Wifi invitados, IOT, Alexa, etc...)

\- Para el **punto de acceso** hay bastante mercado, ya depende del uso que le des al Wifi. Yo recomendaría calidad/precio un `Tplink EAP 225` o un `Ubiquiti AP AC Lite`

\------------

Os pongo los enlaces de aliexpress para que lo tengáis más facil

[Huawei HG8310M](<https://es.aliexpress.com/item/4000747557354.html?spm=a2g0s.9042311.0.0.274263c0BAHXDX> "Huawei HG8310M")

[Mini PC fanless](<https://es.aliexpress.com/item/1005001794025490.html?spm=a2g0s.9042311.0.0.274263c06IZZI4> "Mini PC fanless")

\------------

Agradecer el gran trabajo de Luis Palacios en sus [apuntes técnicos](<https://www.luispa.com/linux/2014/10/05/router-linux.html> "apuntes técnicos") dedicados a movistar, pues me basé en ellos para montar mi red y hacer esta serie de manuales.