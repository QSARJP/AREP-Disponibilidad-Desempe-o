
# Taller práctico de Disponibilidad y Desempeño

# 2019-2 Arquitectura empresarial Grupo: 1

# Profesor

Luis Daniel Benavides Navarro, Ph.D.

#  Integrante

Juan Pablo Ospina Henao

# Compile and run instructions

Se debe de tener instalado en la maquina donde se desea correr Node.js 

## Problema 

El problema que se va a tratar requiere alto procesamiento computacional ya que es encontrar un  numero de la secuencia de fibonacci

## Depliegue la solución en AWS en EC2
Se sube la correspondiente carptea donde se encuentra el problema con su respectivo codigo.
![](/img/2.png)

La maquina se despliega en el sigueinte [enlace](http://ec2-54-152-95-223.compute-1.amazonaws.com:3000/fibonacci/100)

Como se ve en la sigueinte figura 

![](/img/1.png)

## Configure la máquina servicio

Para que el servicio este disponible cuando se reincie la maquina se debe ejecutar las siguinetes indicaciones.
 
* Debe estar en el directorio fibonacci

    * npm install
    * npm install forever -g
    * para que siempre este en ejecucion forever start FibonacciApp.js 
    
    
## Pasos para la creacion del AMI y solucion por escalonamiento

### AMI Y TEMPLATE

Antes de crear un AMI es necesario tener una plantilla de lanzamiento para ello vamos a la consola y buscamos launch templates

![](/img/3.png)

Cuando estemos configuranto este template, se debe elegir una version del AMI, como este nos sirve para la elasticidad de la maquina principal ec2. en la sigueinte foto seleccionamos amazon linuz 2 ya que es el servicio gratis.

![](/img/4.png)

Pasamos a configurar el vpc que tenga definido el puerto que utilzara la aplicaciones para este caso es el 3000 y el id del grupo se muestra en la sigueinte imagen.
![](/img/5.png)

 Y finalizamos la creacion del tempalte.

 ### CONFIGURACION DE LANZAMIENTO 

![](/img/6.png)

Se pasa luego a configurar el autoescalameinto, en este paso debemos elegri nuevmaiente el AMI como se indica en la sigueinte iamgen.

![](/img/7.png)
 y configurar el lanzamineto con su respectivo configuraciòn, es muy importate garatizar la ip por cada instancia.

![](/img/8.png)

Como el vpc con sus respectipos inbound.

![](/img/9.png)

Para esto nos pide una key, para este caso seleccioaremos la misma key que la que se creo el primer recurso ec2

![](/img/10.png)

### Group AutoScaling

Se debe tambien configurar el grupo de lanzamiento, con la finalidad de garantizar el lugar de creacion de las maquinas.

![](/img/11.png)

Asi se puede ver mas detallado la configuracion del grupo

![](/img/13.png)

## politica de auto escalamiento 



en el grupo de escalamiento, se pueden realizar politicas, con la finalidad de que el escalonamiento se pueda realizar automaticamente sin necesidad de crear una nueva isntancia, para un ejemplo se creo la sigueinte politca

![](/img/12.png)


## Cargue la solución con muchas solicitudes para generar alta carga en el servidor

Con el fin de realizar la prueba, es necesario hacer peticiones concurrentes al servidor para ello usamos el sigueinte codigo:

![](/img/14.png)

Principio:

se tiene dos instancias y con la aterior politca se probaran los siguientes casos
 

 * con 100 peticion hallando el valor 1000 de fibonacci

La primera instancia tiene la mayor carga entre las dos, resolviendo el mayor numero de peticiones.
![](/img/15.png)

Instancia 2 
![](/img/16.png)


* con 1000 peticion hallando el valor 1000 de fibonacci
Para este caso habian 3 isntancias las cuales podemos ver el rendimiento de cada una de las maquinas, donde una es la que predomina con respecto a las otras.

![](/img/18.png)
* con 10000 peticion hallando el valor 1000 de fibonacci

Para este caso se habilita una nueva instancia, quedarina en total 4. de las cuales auqnue se ve que solo predomina una, hay una instacia que esta creciendo considerablemente.

![](/img/19.png)

* con 100000 peticion hallando el valor 1000 de fibonacci

y en la ultima prueba vemos como se ve una mayor participacion por cada de las maquinas.

![](/img/20.png)

Esto se da gracias al escalonamiento horizontal y para tener un recuento de como se esta comportanto el grupo de escalonmiento en la siguiente imagen se ve la trayectoria del grupo:

![](/img/17.png)



