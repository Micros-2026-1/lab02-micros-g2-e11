[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/KzqfxGd5)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=22828362&assignment_repo_type=AssignmentRepo)
# Lab02 - Caracterización de osciladores (externo vs. interno)

# Objetivos:
1 Configurar el PIC18F45K22 (o la referencia de PIC seleccionada) para operar con:

- Oscilador externo basado en cristal de cuarzo.

- Oscilador interno (INTOSC).

1.2 Verificar la frecuencia real de la CPU generando una señal de referencia en un pin de salida y midiendo dicha señal con un osciloscopio.

1.3 Comparar ambos modos de operación (cristal vs. INTOSC) en cuanto a:

- Precisión absoluta de la frecuencia.

- Estabilidad temporal de la señal.

- Efectos de la deriva térmica sobre la frecuencia.

## 1. Integrantes
# [Jose Angel Figueroa Castañeda ](https://github.com/joseanfigueroaca)

# [Diego Fernando Yugue Robayo](https://github.com/DiegoYugue)

# [Nicolas Esteban Martines Rocha](https://github.com/nicolasesmartinezro)

## 2. Documentación
[PICK18F45K22] (https://www.alldatasheet.com/datasheet-pdf/download/348759/MICROCHIP/PIC18F45K22.html)
## Procedimiento
1. *Aceptar la tarea* en GitHub Classroom: [ENLACE A LA TAREA]  
2. Abrir *MPLAB X* y crear un proyecto para *PIC18F45K22*.  
3. Con el código base de Classroom, crear main.c  en *Source Files*.  
4. *Realizar el montaje* mostrado en la figura (ver sección 6).  
5. *Compilar* y *grabar* el programa en el PIC (PICkit 3/4).  
6. *Observar* la onda generada por el oscilador interno .  
7. *Modificar* colocar el cristal de cuarso y medir la onda del oscilador externo  
8. *Modificar* colocar el cristal de cuarso y medir la onda del oscilar en la salida 15 del pick 

### 2.1 Descripción del laboratorio

### 2.2 Explicación del código implementado

### 2.3 Análisis y comparación

#### Tabla 1: Medición en frío

| Modo de oscilador | Freq. teórica Fosc | RA6 medible (CLKO)? | Freq. medida RA6 (Hz) | Freq. teórica RC0 (Hz)| Freq. medida RC0 (Hz) | Error RC0 (%) |  
|------------------|------------------|---------------------|---------------|---------------------|---------------|---------------|
| INTOSC (interno) | 16,000,000       | Sí                 |                     |                500                 |               |               | |
| HS (cristal externo 16 MHz) | 16,000,000 | No |     NA      |               500                 |               |               |
| RC externo       | ~16,000,000*     | No                                    |       N/A        | 500                 |               |               | |

#### Tabla 2: Medición con calor

| Modo de oscilador | Freq. teórica Fosc | RA6 medible (CLKO)? | Freq. medida RA6 (Hz) | Freq. teórica RC0 (Hz)| Freq. medida RC0 (Hz) | Error RC0 (%) |  
|------------------|------------------|---------------------|---------------|---------------------|---------------|---------------|
| INTOSC (interno) | 16,000,000       | Sí                 |                     |                500                 |               |               | |
| HS (cristal externo 16 MHz) | 16,000,000 | No |     NA      |               500                 |               |               |
| RC externo       | ~16,000,000*     | No                                    |       N/A        | 500                 |               |               | |

#### Tabla 3: Deriva

| Modo de oscilador |RC0 deriva (Hz) |
|------------------|--------------------|
| INTOSC (interno) |                    |                
| HS (cristal externo 16 MHz) |                |                |
| RC externo       |                 |                


<!-- Agregar tablas para valores usando PLL -->

<!-- Complemente con análisis de lo registrado en tablas -->

## 2.4 Diagramas
- oscilador interno 
👉[ver diagrama ](image-2.png)

- oscilador externo 
👉[ver diagrama ](image-3.png)


## 2.5 Formas de onda 
- Oscilador interno nos muestra una onda cuadrada 
- Oscilador externo a la salida del pck 15 nos da tambien una onda cuadrada 
- Oscilador externo en el cristal de cuarzo nos da una onda aleta de tiburon 



### INTOSC (interno) 


### HS

## RC

## 3. Evidencias de implementación
*Procedimiento*
- Onda oscilador interno 
👉[ver diagrama ](image-6.png)

- Onda oscilador externo 
👉[ver diagrama ](image-4.png)

- Onda oscilador externo pin 15 
👉[ver diagrama ](image-5.png)

## 4. Preguntas

* ¿En qué modo se obtuvo la medición más cercana a la frecuencia teórica?

- La medición más cercana a la teórica siempre se obtiene en el Modo 2 (Cristal de Cuarzo externo).

El porqué: Los cristales de cuarzo son componentes electromecánicos fabricados con una precisión altísima (su error se mide en partes por millón, o ppm). Si usaste un cristal de 16 MHz, la frecuencia real que verás en el osciloscopio será de 15.999 MHz o algo extremadamente cercano, siempre y cuando los capacitores estén correctos.

* ¿Fue posible evidenciar el fenómeno de deriva? ¿Qué factores podrían explicar la variación de frecuencia al calentar el PIC?

- Sí, es posible evidenciarlo, especialmente en el Modo 1 (Oscilador Interno) o Modo 3 (RC Externo). Al ver la señal en el osciloscopio, la onda se "ensancha" o "encoge" ligeramente (cambia su frecuencia) cuando el chip se calienta.

- Factores que lo explican: El oscilador interno del PIC es básicamente un circuito RC microscópico dentro del silicio. La resistencia y la capacitancia de estos componentes internos son termosensibles. Cuando la temperatura aumenta, la agitación térmica de los electrones altera el valor de la resistencia y la capacitancia interna, lo que cambia la velocidad a la que se carga y descarga el circuito, causando la "deriva" o desviación de la frecuencia.

* ¿Cuál es más preciso en cuanto a frecuencia teórica vs. medida?

- Definitivamente el Modo 2 (Cristal de Cuarzo).
Mientras que el oscilador interno (Modo 1) tiene un margen de error de fábrica que puede rondar entre el $\pm 1\%$ y el $\pm 2\%$ a temperatura ambiente (¡eso es un error de hasta 320 kHz en un reloj de 16 MHz!), el cristal de cuarzo suele tener una precisión superior al $0.005\%$.

* Explique cómo usar RC0 para estimar la frecuencia del oscilador cuando RA6 no está disponible.

- En el código, programamos que RC0 cambie de estado cada 1 milisegundo usando __delay_ms(1). Esto debería generar una onda cuadrada teórica con un periodo de $T = 2 \text{ ms}$ (frecuencia de 500 Hz).- El compilador calcula ese retardo basándose en la constante _XTAL_FREQ que tú le diste (ej. 16 MHz).
- Conectas el osciloscopio a RC0 y mides el periodo real de la onda.
- Si el oscilador del PIC está yendo más lento o más rápido que los 16 MHz teóricos, el tiempo del delay se alargará o acortará proporcionalmente.
- Puedes estimar la frecuencia real del oscilador usando una regla de tres simple:$$F_{real} = F_{teorica} \times \left( \frac{T_{teorico}}{T_{real}} \right)$$

* Si se quisiera duplicar la frecuencia del PIC usando PLL, ¿en qué modos se podría aplicar?

- El PLL solo se puede aplicar en el Modo 1 (Oscilador Interno) y el Modo 2 (Cristal Externo / HS / XT).
El Modo 3 (RC Externo) es demasiado inestable y ruidoso, por lo que el circuito multiplicador del PLL no logra "engancharse" a la señal y no se permite su uso.

* Enliste ventajas y desventajas de cada modo.

1. Modo 1: Oscilador Interno

En este modo, el microcontrolador utiliza un circuito RC (Resistencia-Capacitor) miniaturizado que ya viene fabricado dentro del propio chip de silicio.
 
 * Ventajas: 
 - Ahorro de espacio y dinero: No necesitas comprar ni conectar ningún componente externo en la protoboard.
 - Libera pines: Los pines OSC1 y OSC2 (que normalmente se usan para el reloj) quedan libres y puedes usarlos como pines de entrada o salida digital normales para conectar más LEDs o sensores.
 - Arranque rápido: El oscilador interno "despierta" y se estabiliza casi instantáneamente al encender el circuito.

 * Desventajas:
 - Menor precisión: Tiene un margen de error de fábrica (usualmente entre $\pm 1\%$ y $\pm 2\%$). Si necesitas 16 MHz exactos, puede que en realidad te esté dando 15.8 MHz o 16.2 MHz.
 - Deriva térmica: Como vimos antes, es muy sensible a los cambios de temperatura y variaciones en el voltaje de alimentación. Si el chip se calienta, la frecuencia cambia.
 -No apto para comunicaciones críticas: Por su falta de precisión, puede dar problemas si intentas usar protocolos de comunicación estrictos como USB, CAN bus o a veces UART a altas velocidades.

 2. Modo 2: Cristal Externo (HS / XT)
En este modo, conectas un cristal de cuarzo físico junto con dos pequeños capacitores cerámicos a los pines OSC1 y OSC2.

* Ventajas: 
- Precisión extrema: Es el modo más exacto de todos. Su error es minúsculo (se mide en partes por millón o ppm).
- Alta estabilidad: La frecuencia casi no cambia aunque el ambiente se caliente o el circuito lleve horas encendido. Es inmune a la deriva térmica severa.
- Obligatorio para tareas exactas: Es el estándar de oro si necesitas llevar el tiempo real (como un reloj de pared) o comunicarte con la computadora sin perder datos.

*  Desventajas:

- Ocupa pines: Pierdes irremediablemente los dos pines del oscilador (OSC1 y OSC2), por lo que tienes menos entradas/salidas disponibles.
- Requiere más hardware: Tienes que agregar el cristal y los capacitores, lo que ocupa espacio en tu placa.
- Sensible al diseño físico: Como sufriste en la protoboard, el ruido eléctrico, los cables largos o la "capacitancia parásita" pueden ahogar al cristal y evitar que arranque.

3. Modo 3: RC Externo (Resistencia-Capacitor)
Aquí construyes tu propio reloj conectando una resistencia (R) y un capacitor (C) básicos al pin OSC1. La velocidad dependerá de los valores que elijas para esos dos componentes.
* Ventajas:
- Costo extremadamente bajo: Las resistencias y capacitores estándar cuestan centavos.
- Frecuencia personalizable: Puedes obtener frecuencias "raras" o específicas simplemente cambiando la resistencia o el capacitor, sin tener que buscar un cristal fabricado a esa medida exacta.

* Desventajas:

- Pésima precisión: Es el modo más impreciso de todos. Los componentes comunes tienen tolerancias muy altas (una resistencia puede variar un $5\%$ de su valor real y un capacitor hasta un $20\%$).
- Altamente inestable: El ruido ambiental, la temperatura del cuarto y el voltaje afectan drásticamente a los componentes externos, haciendo que la frecuencia baile de un lado a otro constantemente.
- Limitado en velocidad: No es recomendable para frecuencias altas (como 16 MHz), se suele usar solo para velocidades bajas donde el tiempo no es un factor crítico.
## 5. Referencias