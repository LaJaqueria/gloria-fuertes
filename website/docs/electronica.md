# Eletrónica

- Tiras de LEDs de 20m -> [link](https://es.aliexpress.com/item/1005003741393424.html)
- Placas ESP32 C3 Super Mini  + Shield expansion board -> [link](https://es.aliexpress.com/item/1005006345128364.html)

## LEDs - Alimentación

- Se han pedido 4 tiras de leds de 20m -> 80m de tiras
- Cada metro de tira contiene 10 leds -> 1 led cada 10cm
- Cada led consume 0.1 - 0.3 W
- Cada 1m leds === 1 - 3 W
- Tira de 20m leds === 20 - 60 W
- Las tiras se pueden cortar y empalmar a nuestro antojo

Para calcular la potencia también WLED nos provee de un recursos para eso

- Calculador de potencia para los leds con WLED => [link](https://wled-calculator.github.io/)

## Placas ESP32 C3 Super Mini

Usamos las placas ESP32 C3 Super Mini porque son bastante pequeñas, tienen todas las ventajas de las placas ESP32 (WiFi, Bluetooth, etc) y además tienen USB-C.

### Flashear placas con WLED

Para poder usar estas placas primero hay que flashearlas con el software WLED

1. Conecta la placa a tu ordenador con un cable USB-C
2. Abre Google Chrome (no vale otro navegador)
3. Entra en la web [https://install.wled.me/](https://install.wled.me/)
4. Selecciona la versión del firmware a cargar
5. Seleccion "Plain" y luego dale a "Install"
6. Deberás elegir el puerto USB donde está conectada la placa
7. Configurale el WiFi y demas
8. Luego te dirá en que IP está, entra en ella, vete a "Config" y termina de configurar toda la parte de red.
