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

- Estabilidad temporal de la señal. ybotra cosa

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
El presente laboratorio tiene como propósito principal implementar y comprobar el funcionamiento de tres diferentes fuentes de oscilación (reloj) en un microcontrolador de la familia PIC18F (específicamente el PIC18F22K), haciendo uso del programador PICkit 3.5. Durante la práctica, se configurará el microcontrolador para operar bajo tres esquemas distintos:

Modo interno: Utilizando el oscilador integrado del propio chip, lo cual ahorra componentes externos.

Modo cristal: Conectando un cristal de cuarzo externo para lograr una alta precisión en los tiempos de ejecución.

Modo RC: Construyendo un reloj externo básico mediante un circuito de resistencia y condensador (RC).

A través de esta implementación, se busca comprender cómo se configura cada fuente de reloj a nivel de hardware y software, y observar cómo cada modo afecta directamente la velocidad de trabajo y la precisión del microcontrolador.
### 2.2 Explicación del código implementado
Este código está escrito en C (utilizando el compilador XC8, indicado por #include <xc.h>) y está diseñado para un microcontrolador PIC. Su objetivo principal es configurar el hardware para evaluar diferentes tipos de osciladores y generar una señal cuadrada de 500 Hz en un pin de salida.

1. Configuración General (#pragma config)
Esta sección prepara el "terreno" del microcontrolador apagando funciones que podrían interferir con tu experimento:

WDTEN, LVP, BOREN, etc. en OFF: Apaga el Perro Guardián (Watchdog Timer), la programación en bajo voltaje, los reinicios por caídas de tensión y las protecciones de código. Esto garantiza que el microcontrolador no se reinicie inesperadamente mientras haces tus mediciones.

2. Selección del Modo de Oscilador (#define MODE 1)
El código utiliza directivas de preprocesamiento (#if, #elif) para que puedas cambiar fácilmente entre los tres modos de oscilador que evaluaste en tus tablas, cambiando un solo número:

Modo 1 (INTIO67): Usa el oscilador interno del microcontrolador.

Modo 2 (HSHP): Usa un cristal de cuarzo externo de alta velocidad (High-Speed).

Modo 3 (RC): Usa un circuito de resistencia-condensador externo.

Actualmente, el código está configurado para compilarse en el Modo 1 (#define MODE 1).

3. Frecuencia del Sistema (_XTAL_FREQ)
Esta macro es crucial porque le dice al compilador a qué velocidad está corriendo el microcontrolador para que la función __delay_ms() calcule los tiempos correctamente.

Observación importante: Si te fijas, para el Modo 1 sin PLL (como está configurado ahora), el código define _XTAL_FREQ como 900000UL (900 kHz). Sin embargo, en las tablas que me mostraste anteriormente, tu frecuencia teórica era de 16 MHz. Si el hardware está configurado a 16 MHz pero el software cree que está a 900 kHz, los retardos no serán precisos.

4. Funciones de Inicialización
init_pins(): Configura los pines de entrada/salida.

Establece el pin RC0 como salida digital (poniendo TRISC0 = 0) y lo inicializa apagado (LATC0 = 0).

También configura el pin RA6 como salida si el modo lo permite (por ejemplo, para medir la señal de reloj o CLKO, tal como se menciona en tus tablas).

init_oscillator(): Activa el multiplicador de frecuencia (PLL) en los registros del microcontrolador, pero solo si la variable USE_PLL está activada (actualmente está en 0).

5. El Bucle Principal (main)
Después de inicializar los pines y el oscilador, el programa entra en un bucle infinito (while(1)):

Enciende el pin RC0 (LATC0 = 1).

Espera 1 milisegundo.

Apaga el pin RC0 (LATC0 = 0).

Espera 1 milisegundo.

¿Por qué genera 500 Hz?
Un ciclo completo de la señal (encendido y apagado) tarda 2 milisegundos. La frecuencia es el inverso del período (1 / 0,002 segundos), lo que da exactamente 500 Hz. Esta es la "Freq. teórica RC0" que aparece en tus tablas de resultados.

## Código Optimizado
Basado en el análisis anterior, el código de producción se configura permanentemente para el modo **HS (Cristal Externo)** por ser el más estable. Además, se define correctamente `_XTAL_FREQ` a 16 MHz para garantizar la precisión de los retardos en todo momento.

```c
#include <xc.h>    
#include <stdint.h>

// ========================== CONFIGURACIÓN GENERAL ========================
#pragma config WDTEN = OFF      
#pragma config LVP = OFF        
#pragma config PBADEN = OFF     
#pragma config CP0 = OFF, CP1 = OFF, CP2 = OFF, CP3 = OFF  
#pragma config BOREN = OFF      
#pragma config FCMEN = OFF      
#pragma config IESO = OFF       

// ========================== MODO DE OSCILADOR ==========================
// Configurado permanentemente para el cristal externo (El más preciso)
#define MODE 2 

#if MODE == 1
    #pragma config FOSC = INTIO67   
    #define USE_PLL 0
#elif MODE == 2
    #pragma config FOSC = HSHP     
    #define USE_PLL 0
#elif MODE == 3
    #pragma config FOSC = RC       
    #define USE_PLL 0
#else
    #error "Modo de oscilador inválido"
#endif

// ========================== FRECUENCIA DEL OSCILADOR =====================
// Definido a 16 MHz teóricos para cálculos exactos de delay
#define _XTAL_FREQ 16000000UL 

// ========================== FUNCIONES ==========================
void delay_ms(uint16_t ms) {
    while(ms--) {
        __delay_ms(1); 
    }
}

void init_pins(void) {
    TRISCbits.TRISC0 = 0;
    LATCbits.LATC0 = 0;

    if(MODE == 1 || (MODE == 2 && USE_PLL)) {
        TRISAbits.TRISA6 = 0;
        LATAbits.LATA6 = 0;
    }
}

void init_oscillator(void) {
#if USE_PLL
    OSCCONbits.SPLLEN = 1;  
#endif
}

// ========================== PROGRAMA PRINCIPAL ==========================
void main(void) {
    init_pins();
    init_oscillator();

    while(1) {
        // RC0 toggle a 500 Hz
        LATCbits.LATC0 = 1;
        delay_ms(1);
        LATCbits.LATC0 = 0;
        delay_ms(1);
    }
}
```
## 2.3 Análisis y comparación

Basado en las mediciones empíricas en frío y con calor de la señal en RC0 frente a los 500 Hz teóricos, se obtuvieron las siguientes conclusiones para cada modo de oscilador:

1. Modo INTOSC (Oscilador Interno)
Comportamiento: Mostró una buena precisión en frío (496,35 Hz), pero demostró ser muy vulnerable a los cambios de temperatura. Al aplicarle calor, la frecuencia cayó drásticamente a 487,54 Hz (error del 2,56%).

Uso recomendado: Ideal para proyectos básicos donde el ahorro de espacio y pines es prioridad, pero no recomendado si el dispositivo va a sufrir estrés térmico o requiere tiempos exactos.

2. Modo HS (Cristal de Cuarzo Externo) 
Comportamiento: Fue el ganador indiscutible del experimento. Mantuvo la frecuencia altamente estable en ambas condiciones (496,27 Hz), registrando el margen de error más bajo de la prueba (~0,75%). Demostró ser prácticamente inmune a las variaciones térmicas.

Uso recomendado: Obligatorio para aplicaciones profesionales, medición de tiempos exactos, relojes en tiempo real y comunicación serial estricta.

3. Modo RC Externo (Resistencia-Capacitor Externos)
Comportamiento: Su rendimiento fue deficiente frente a los cambios de temperatura. Aunque en frío logró 496,1 Hz, al aplicarle calor la frecuencia cayó a 489 Hz (error del 2,25%). Además, su frecuencia inicial depende enteramente de la tolerancia física de la resistencia y el capacitor utilizados.

Uso recomendado: Prácticamente obsoleto en la actualidad debido a la superioridad técnica, menor costo y ahorro de pines que ofrecen los osciladores internos modernos.




#### Tabla 1: Medición en frío

| Modo de oscilador | Freq. teórica Fosc | RA6 medible (CLKO)? | Freq. medida RA6 (Hz) | Freq. teórica RC0 (Hz)| Freq. medida RC0 (Hz) | Error RC0 (%) |  
|------------------|------------------|---------------------|---------------|---------------------|---------------|---------------|
| INTOSC (interno) | 16,000,000       | Sí                 |         31,4            |                500                 |       496,35        |       0,93        | |
| HS (cristal externo 16 MHz) | 16,000,000 | No |     NA      |               500                 |       496,27        |        0,746       |
| RC externo       | ~16,000,000*     | No                                    |       N/A        | 500                 |        496,1       |        0,78       | |

#### Tabla 2: Medición con calor

| Modo de oscilador | Freq. teórica Fosc | RA6 medible (CLKO)? | Freq. medida RA6 (Hz) | Freq. teórica RC0 (Hz)| Freq. medida RC0 (Hz) | Error RC0 (%) |  
|------------------|------------------|---------------------|---------------|---------------------|---------------|---------------|
| INTOSC (interno) | 16,000,000       | Sí                 |                     |                500                 |       487,54        |      2,56         | |
| HS (cristal externo 16 MHz) | 16,000,000 | No |     NA      |               500                 |       496,27        |         0,75      |
| RC externo       | ~16,000,000*     | No                                    |       N/A        | 500                 |        489       |        2,25       | |

#### Tabla 3: Deriva

| Modo de oscilador |RC0 deriva (Hz) |
|------------------|--------------------|
| INTOSC (interno) |          16MHz*4          |                
| HS (cristal externo 16 MHz) |         900KHz       |                |
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


### INTOSC 
<p align="center">
  <img src="https://github.com/user-attachments/assets/3045b3af-8eb5-45fc-ab6d-267bb5cf8fec" width="50% shadow="true"">
  <br>
  <em>ONDA OSCILOSCOPIO MODO INTERNO</em>
</p>


### HS
<p align="center">
  <img src="https://github.com/user-attachments/assets/e4f04b98-409f-4175-ae16-9cacb8582164" width="50% shadow="true"">
  <br>
  <em>ONDA OSCILOSCOPIO MODO EXTERNO</em>
</p>

## RC
<p align="center">
  <img src="https://github.com/user-attachments/assets/aaf2fd6d-7002-41c9-9821-8e66db7dc04a" width="50% shadow="true"">
  <br>
  <em>ONDA OSCILOSCOPIO MODO RC</em>
</p>

## SIMULACION GENERAL
<p align="center">
  <img src="https://github.com/user-attachments/assets/8f4b4485-16a4-423b-af52-afb57cfddc4d" width="50% shadow="true"">
  <br>
  <em>PROTEUS</em>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/78f8fffa-2454-4620-aae4-4c9a0d8df90c" width="50% shadow="true"">
  <br>
  <em>GRAFICA</em>
</p>

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

-Depende de la condición de temperatura, pero si miramos estrictamente los números crudos de la columna "Freq. medida RC0 (Hz)" frente a los 500 Hz teóricos:

En frío (Tabla 1): El modo INTOSC (interno) registró 496,35 Hz. Esta es la medición absoluta más cercana a 500 Hz de ambas tablas (una diferencia de solo 3,65 Hz).

Con calor (Tabla 2): El modo HS (cristal externo) registró 496,27 Hz, siendo el más cercano en esta condición, ya que los otros modos cayeron drásticamente (a 487,54 Hz y 489 Hz).

* ¿Fue posible evidenciar el fenómeno de deriva? ¿Qué factores podrían explicar la variación de frecuencia al calentar el PIC?

- Sí, es posible evidenciarlo, especialmente en el Modo 1 (Oscilador Interno) o Modo 3 (RC Externo). Al ver la señal en el osciloscopio, la onda se "ensancha" o "encoge" ligeramente (cambia su frecuencia) cuando el chip se calienta.

- Factores que lo explican: El oscilador interno del PIC es básicamente un circuito RC microscópico dentro del silicio. La resistencia y la capacitancia de estos componentes internos son termosensibles. Cuando la temperatura aumenta, la agitación térmica de los electrones altera el valor de la resistencia y la capacitancia interna, lo que cambia la velocidad a la que se carga y descarga el circuito, causando la "deriva" o desviación de la frecuencia.

* ¿Cuál es más preciso en cuanto a frecuencia teórica vs. medida?

-El modo HS (cristal externo 16 MHz) es indiscutiblemente el más preciso y, sobre todo, el más estable.

Menor desviación: Si nos guiamos por la columna de Error (%), el modo HS tiene el error más bajo registrado: 0,746%.

Inmunidad a la temperatura: Su precisión se mantiene prácticamente intacta sin importar la temperatura (0,746% en frío frente a 0,75% con calor).

Contraste: Por el contrario, los modos INTOSC y RC externo demuestran ser muy imprecisos bajo estrés térmico, disparando su margen de error a 2,56% y 2,25% respectivamente cuando se les aplica calor.

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
https://www.youtube.com/watch?v=duRDGrotWjQ
https://www.youtube.com/watch?v=27fmSFTqeg0
https://www.todopic.com.ar/foros/index.php?topic=30693.0
https://www.youtube.com/watch?v=PhbIlsvHE9g
