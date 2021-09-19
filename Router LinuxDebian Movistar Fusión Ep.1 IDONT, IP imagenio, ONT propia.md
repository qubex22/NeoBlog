#### #### Esta serie de posts va a tratar de explicar cómo quitar por completo el router de movistar, sustituyendolo por una ONT propia y un router basado en linux. En este primer capítulo explicaré cómo averiguar el IDONT, necesario para colocar nuestra ONT, y la ip estática de la red de imagenio que necesitaremos más adelante para configurar el router

1\. Nos conectamos al router de movistar y entramos a la página de configuración `http://192.168.1.1` , una vez dentro vamos a **configuración avanzada/wan interface** y pinchamos en la **interfaz con la vlan 2** y que tiene ip estática. Es la interfaz del servicio de TV, **apuntamos la ip, máscara y gateway**.

![](<https://i.postimg.cc/50JQzSkH/01.png>)

![](<https://i.postimg.cc/1X5gC5Ss/02.png>)

![](<https://i.postimg.cc/63h2f4DM/03.png>)

2\. Ahora para sacar el **IDONT** accedemos a esta url `http://192.168.1.1/index_instalacion.asp` y lo copiamos

![](<https://i.postimg.cc/WpXqDrm3/04.png>)

3\. En este momento nos conectamos a nuestra **ONT** (las de **ubiquiti NO VALEN para imagenio**, no permiten multicast) , en mi caso (y la recomiendo, se encuentra barata en aliexpress, y la configuración es muy sencilla) **Huawei HG310M**. Y pegamos el IDONT. En este caso seleccionamos Password y Hex. String, copiamos el idont tal cual está en password y en SN lo copiamos y rellenamos de 0 delante. (si tienes otra ONT será similar pero no igual, aun así suelen ser intuitivas para meter el idont)

![](<https://i.postimg.cc/tRZ7gfgr/05-LI.jpg>)