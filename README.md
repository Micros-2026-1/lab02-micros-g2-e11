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

# [Nicolas Esteban Martines Rocha](https://github.com/NicolasMartinez)

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
👉[ver diagrama ](image.png)

-oscilador externo 
👉[ver diagrama ](image-1.png)


## 2.5 Formas de onda

### INTOSC (interno) 


### HS

## RC

## 3. Evidencias de implementación

## 4. Preguntas

* ¿En qué modo se obtuvo la medición más cercana a la frecuencia teórica?

* ¿Fue posible evidenciar el fenómeno de deriva? ¿Qué factores podrían explicar la variación de frecuencia al calentar el PIC?

* ¿Cuál es más preciso en cuanto a frecuencia teórica vs. medida?


* Explique cómo usar RC0 para estimar la frecuencia del oscilador cuando RA6 no está disponible.

* Si se quisiera duplicar la frecuencia del PIC usando PLL, ¿en qué modos se podría aplicar?

* Enliste ventajas y desventajas de cada modo.

## 5. Referencias