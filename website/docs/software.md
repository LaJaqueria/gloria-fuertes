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
