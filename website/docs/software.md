# Software

Libreria para usar los leds, WLED -> [link](https://kno.wled.ge/)

## Como empezar

Primero deberás tener las placas ESP32 C3 flasheadas -> [mira aquí](electronica.md#flashear-placas-con-wled)

En este repositorio tienes una carpeta `src` que contiene un programa Node-Red dockerizado para poder levantar el programa.

## Interactuar con los LEDS

Se recomienda entrar en la pagina web del propio cacharro y fijar en la config la parte técnica de:

- Wifi, ip statica, etc
- Modelos de Leds usados, potencia, rgb, etc
- Definir cuantos leds hay (200s por tira de 20m) y en que patilla estan (normalmente usamos GPIO2)

Para mandarles ordenes tiene una API Rest a partir del endpoint /json

GET -> IP/json => Devuelve un JSON con 4 campos, 3 de ellos son readonly y solo uno sirve para interactuar:

- state -> Unico campo para interactuar
- effects -> Array de nombres de los efectos
- palettes -> Array de nombres de las paletas
- info -> Información física de la placa (IP, nivel de señal wifi, version firmware, etc)

Se pueden obtener cada uno por separado haciendo una peticion GET

- state -> GET IP/json/state
- effects -> GET IP/json/effects
- palettes -> GET IP/json/palettes
- info -> GET IP/json/info

### Mandar cosas a las tiras de leds

Hay que mandar un json con el state que tiene que tener toda la tira

POST IP/json/state

Ese json es de esta forma

```typescript
type state = {
    // Para toda la tira -> Enciende true | Apaga false | Toggle "t",
    on: boolean | "t",
    // Para toda la tira -> Brillo de 0 a 255
    bri: int8,
    // Para cada segmento -> Array con los estados de cada segment
    seg: Array<{
        // El led donde empieza, el mas pequeño es 0
        start: int,
        // El led donde acaba, se puede omitir si se manda len
        end: int,
        // Longitud del segmente, se puede omitir si se manda end
        len: int,
        // Array con los colores
        // Tiene 3 elementos: color primario, secundario y terciario
        // Solo importa el primero
        // Cada elemento a su vez es un array de 3 elementos -> RGB de 0 a 255
        col: Array<[red: int8, green: int8, blue: int8]>[3]
        // Efecto del segmento -> Es el índice (numero) del array de /json/effects
        fx: int,
        // Encender apagar el segmento -> Enciende true | Apaga false | Toggle "t",
        on: boolean | "t",
        // Brillo sel segment -> Brillo de 0 a 255
        bri: int8,
    }>
}
```
### Un ejemplo de uso de curl para cambios de estado rapidos
desde un equipo configurado en la misma wifi que los dispositivos wled, y sabiendo que tienen las IPs 192.168.121.50 y 51:
```
#!/bin/bash
while true
do
curl -X POST "http://192.168.121.50/json/state" -H "Content-Type: application/json" --data "@off.json"
curl -X POST "http://192.168.121.51/json/state" -H "Content-Type: application/json" --data "@off.json"
sleep 0.1
curl -X POST "http://192.168.121.51/json/state" -H "Content-Type: application/json" --data "@on.json"
curl -X POST "http://192.168.121.50/json/state" -H "Content-Type: application/json" --data "@on.json"
sleep 0.1
done
```

los json on y off:

on.json:
```
{"on":true,"bri":255,"transition":0,"ps":-1,"pl":-1,"nl":{"on":false,"dur":60,"mode":1,"tbri":0,"rem":-1},"udpn":{"send":false,"recv":true,"sgrp":1,"rgrp":1},"lor":0,"mainseg":0,"seg":[{"id":0,"start":0,"stop":200,"len":200,"grp":1,"spc":0,"of":0,"on":true,"frz":false,"bri":255,"cct":127,"set":0,"col":[[0,247,255],[0,0,0],[0,0,0]],"fx":0,"sx":128,"ix":128,"pal":0,"c1":128,"c2":128,"c3":16,"sel":true,"rev":false,"mi":false,"o1":false,"o2":false,"o3":false,"si":0,"m12":0}]}
```

off.json:
```
{"on":false,"bri":255,"transition":0,"ps":-1,"pl":-1,"nl":{"on":false,"dur":60,"mode":1,"tbri":0,"rem":-1},"udpn":{"send":false,"recv":true,"sgrp":1,"rgrp":1},"lor":0,"mainseg":0,"seg":[{"id":0,"start":0,"stop":200,"len":200,"grp":1,"spc":0,"of":0,"on":true,"frz":false,"bri":255,"cct":127,"set":0,"col":[[0,247,255],[0,0,0],[0,0,0]],"fx":0,"sx":128,"ix":128,"pal":0,"c1":128,"c2":128,"c3":16,"sel":true,"rev":false,"mi":false,"o1":false,"o2":false,"o3":false,"si":0,"m12":0}]}
```

dentro del propio json se pueden definir los segmentos de la tira, dentro de la entrada:
```
"seg":[...]
```

### usando ledFX como puente entre el audio y los wled

[LedFx](https://www.ledfx.app/) es un software que transforma una señal de audio en un espectáculo de luces led en tiempo real (de la documentacion del proyecto). En nuestro caso, es el interfaz entre la voz de las personas que leen los poemas de GF en voz alta, y la estructura interactiva de leds que sirve de escenario para esa lectura. Puede localizar dispositivos ya configurados con wled en la red local, y controlarlos, simplificando la gestión de las tiras de LEDs. Además, puede controlar dispositivos OSC, por lo que puede ser un elemento interesante para escucha interactiva en shows audiovisuales...
