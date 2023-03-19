# raspberry-sensor-cjmcu-811

Clase para integrar el sensor CJMCU-811 con python en aplicaciones fácilmente con métodos para obtener valores y estructura para generar dinámicamente su tabla con SQLAlchemy.
Mide:

Este repositorio se encuentra disponible aquí:

https://gitlab.com/raupulus/raspberry-sensor-cjmcu811.git

Repositorio con modelo y ejemplos para el sensor VEML6075 que obtiene valores
CO2/TVOC del aire en el ambiente:

- CO2 → Concentración de CO2
- TVOC → Compuestos orgánicos volátiles

Para el funcionamiento del sensor se parte de la librería oficial de adafruit:

https://github.com/adafruit/Adafruit_CircuitPython_CCS811.git

De forma que este repositorio utiliza esa librería y sus dependencias como base
añadiendo otras características que he visto necesarias en mi caso.

La clase CJMCU811 puede funcionar de forma autónoma, aún así también es
extendida por clases hijas para seccionar el tipo de resultado obtenido y
tratarse de forma independiente en aplicaciones que lo implementen.
Por lo tanto puedes usar dicha clase ignorando las clases hijas o puedes
usarlas en conjunto.

## Cuidado

En algunos lugares indica que no se lleva bien en el bus con otros i2c,
personalmente he tenido algunos problemas con varios sensores a la vez y lo he
solucionado capturando la excepción y esperando unos minutos para reinicializar
(en el peor de los casos solo han sido 3-5 veces al día trabajando de forma
seguida tomando lecturas cada 40 segundos)

Por otro lado, lee las indicaciones del fabricante ya que puede ser necesario
trabajar 48h antes de tener datos fiables cuando el sensor es reciente.

## Dependencias

Los entornos donde han sido probados satisfactoriamente utilizaban las
siguientes versiones de las aplicaciones necesarias:

- python 3.7.3
- raspbian, basado en debian 10.1
- bash 5.0.3
- pip3 18.1

Se tiene que instalar la librería de adafruit mediante el gestor de paquetes de
python 3

```bash
pip3 install adafruit-circuitpython-ccs811
```

Además, también es necesario instalar las librerías siguientes:

```bash
pip3 install Adafruit-Blinka
pip3 install adafruit-circuitpython-register
pip3 install adafruit-circuitpython-busdevice
```

También puedes adaptarlo usando entornos virtuales o descargar el repositorio
manualmente desde el enlace indicado al comienzo de este documento.

## Links interesantes relacionados

https://revspace.nl/CJMCU-811

https://www.openhacks.com/uploadsproductos/ccs811_ds000459_4-00.pdf

https://github.com/adafruit/Adafruit_CircuitPython_CCS811

https://blog.jokielowie.com/en/2017/12/niedlugo-pomiar-jakosci-powietrza-bosch-bme680-oraz-ccs811-iaq-tvoc/

## Unidades de medida

- PPM → Partes por millón (1/1000000).
- PPB → Partes por billón (10-9)
- CO2 → Concentración de CO2, normalizado 415.50 en 2019
- TVOC → compuestos orgánicos volátiles totales, que son la cantidad total de los gases emitidos procedentes de toxinas y productos químicos.


## TVOC – efecto en función de la concentración
< 0,2 mg/m³     Sin irritación ni síntomas de malestar
0,2 - 3,0 mg/m³     Es posible que aparezcan irritaciones o síntomas de malestar, si el efecto del cambio se da junto con otros parámetros de exposición
3,0 - 25 mg/m³  La exposición conlleva ciertos efectos, posiblemente dolor de cabeza, si el efecto del cambio se da junto con otros parámetros de exposición.
> 25 mg/m³  Dolor de cabeza. Otros efectos neurotóxicos además del dolor de cabeza

## Formaldehído – efecto en función de la concentración
de 0,05 - 0,125 ppm     Umbral de olor
de 0,01 - 1,6 ppm   Irritación de las mucosas (nariz, faringe) y de los ojos
de 2 - 3 ppm    Picor en la nariz, los ojos y la faringe
de 4 - 5 ppm    Soportable durante unos 30 minutos, aumento del malestar, lagrimeo excesivo
de 10 - 20 ppm  A los pocos minutos de exposición se experimenta un lagrimeo excesivo (que puede perdurar hasta una hora después de la exposición), sensación de asfixia inmediata, tos, escozor intenso en la garganta, la nariz y los ojos
30 ppm  ¡Edema pulmonar tóxico, neumonía, peligro de muerte!

## Sensibilidad a gases

- Alklanet (contains 2-butoxyethanol), very sensitive to this stuff, we use it everywhere in the space to clean working surfaces
- permanent marker: very sensitive to it
- toluene: very sensitive
- acetone: very sensitive
- butanol: sensitive
- butylacetate: sensitive
- butane, it is detected but not impressively so
- chloroform, almost no response!
- dichloro-methane: insensitive
- acetaldehyde: not very sensitive to it
