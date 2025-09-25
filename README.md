# Informe Proyecto corto I: Diseño digital combinacional en dispositivos programables 
### Jiahui Zhong Zhe 
### Ximena Araya Brenes 
## Descripción general 
La primera parte consiste en la implementación de un algoritmo de Hamming que permita identificar y corregir errores en palabras de 4 bits. Se realizó trabajando el código de System Verilog en Visual Studio Code, para luego descargarlo en el circuito armado con la FPGA. Además, se trabajó mediante subsistemas, como lo son aquellos para codificar o decodificar, entre otros. La segunda parte del proyecto es un oscilador de anillo, construido con inversores conectados en serie, con el fin de poder analizar con el osciloscopio la forma de su onda y así determinar el tiempo de subida y de bajada. 
## 1. Funcionamiento y descripción del circuito 
El circuito posee elementos resistivos, los cuales regulan la magnitud de voltaje que reciben el resto de los dispotivios. Luego, posee dos tipos de dip switches: de 4 y 8 bits, estos son los que permiten recibir la plabara de 4 bits para poder implementarla en el código de Hamming y así poder determinar cuántos errores tiene. El código de Hamming (7,4) implementa tanto un codificador como un decodificador para ser capaz de detectar, tanto uno como dos errores, single error detection o double error detection (SED o DED). Por otro lado, el circuito incluye dispositivos de display que permite, como su nombre lo indica, desplegar el resultado obtenido. Finalmente, cuenta con un par de transistores de tipo BJT PNP, para la regulación de tensión. Todos estos dispotivos se encuentran conectados a diferentes pines de la FPGA. 
Se trabajó mediante subsistemas que permitieron segmentar el trabajo en diversas tareas y funciones que en conjunto completan el funcionamiento del algoritmo y el circuito en su totalidad. Los mismos se desglosan a continuación: 
1. Subsistema de lectura y codificación de la palabra transmitida: recibe la plabra de 4 bits de datos y genera la codificación de 8 bits utilizando el código de Hamming, tomando en cuenta los bits de paridad. 
2. Subsistema de lectura y decodificación de la palabra recibida: lee los 8 bits que posiblemente contienen errores y separa lo bits de chequeo o paridad de los bits de información.
3. Subsistema verificador de paridad y detector de error: compara los bits de chequeo para determinar si existe SED o DED, así como la posición del error si la hubiese. 
4. Subsistema de corrección de error sobre la palabra recibida: corrige el bit erróneo mediante el síndrome.
5. Subsistema de despliegue de la palabra corregida en formato binario en luces LED: muestra la palabra corregida en los LEDs. 
6. Subsistema de despliegue de palabra corregida en display de 7 segmentos: convierte la palabra corregida a formato hexadecimal para que se pueda visualizar en el display. 
## 2. Diagramas de bloque 
Como se mencionó anteriormente, el trabajo fue realizado mediante la implementación de subsistemas, para ello, se utilizaron diagramas de bloques los cuales optimizan la visualización del flujo de tareas en el orden en el que deben realizarse. Además, facilita la identifiación de entradas y salidas en el sistema. En la figura 1 se muestra el diagrama de bloques implementado. 

![Diagrama de bloques](images/diagrama.png) 

*Figura 1: Diagrama de bloques*

El sistema del codificador de Hamming (7,4) recibe 4 bits de información (i3,i2,i1,i0), luego genera los bits de chequeo (c2,c1,c0), en el bloque de SED o DED calcula el síndrome (s4,s2,s1) para finalmente mostrar los resultados enb los LEDs y el display de 7 segmentos a través de un multiplexor. 
## 3. Ecuaciones booleanas de correción de error
Para poder ejecutar el circuito con SED y DED, es necesario optimizar las ecuaciones booleanas que determinan, tanto la posición del error, como su correción, además, la simplificación de las mismas permite reducir el número de compuertas lógicas necesarias, y así mejorar su eficiencia. 
1. Código de Hamming:
   
c2 = i3 ⊕ i2 ⊕ i1

c1 = i3 ⊕ i2 ⊕ io  

c0 = i3 ⊕ i4 ⊕ i0  

2. Decodificador:
   
c2 = i3 ⊕ i2 ⊕ i1 ⊕ c2

c1 = i3 ⊕ i2 ⊕ io ⊕ c1

c0 = i3 ⊕ i1 ⊕ i0 ⊕ c0

3. Verificador de error y detector de error:

Síndrome: s1,s2,s4

s1 = c0 ⊕ i0 

s2 = c1 ⊕ i1

s3 = c2 ⊕ i3 

Paridad total = c0 ⊕ c1 ⊕ c2 ⊕ i0 ⊕ i2 ⊕ i3 ⊕ i1 

## 4. Ecuaciones booleanas de despliegue de palabras corregidas con luces LED
A través de las ecuaciones booleanas y simplificación mediatne mapas de Karnaugh se optimiza el funcionamiento del display, el cual requiere una función específico que convierte el código binario de 4 a la representación hexadecimal correspondiente. 

Despliegue de la palabra corregido: 

| D3,0 	| Sa 	| Sb 	| Sc 	| Sd 	| Se 	| Sf 	| Sg 	|
|---	|---	|---	|---	|---	|---	|---	|---	|
| 0000 	| 1 	| 1 	| 1 	| 1 	| 1 	| 1 	| 0 	|
| 0001 	| 0 	| 1 	| 1 	| 0 	| 0 	| 0 	| 0 	|
| 0010 	| 1 	| 1 	| 0 	| 1 	| 1 	| 0 	| 1 	|
| 0011 	| 1 	| 1 	| 1 	| 1 	| 0 	| 0 	| 1 	|
| 0100 	| 0 	| 1 	| 1 	| 0 	| 0 	| 1 	| 1 	|
| 0101 	| 1 	| 0 	| 1 	| 1 	| 0 	| 1 	| 1 	|
| 0110 	| 1 	| 0 	| 1 	| 1 	| 1 	| 1 	| 1 	|
| 0111 	| 1 	| 1 	| 1 	| 0 	| 0 	| 0 	| 0 	|
| 1000 	| 1 	| 1 	| 1 	| 1 	| 1 	| 1 	| 1 	|
| 1001 	| 1 	| 1 	| 1 	| 0 	| 0 	| 1 	| 1 	|

Simplificación con Mapa de Karnaugh: 
| i1,i0/i3,i2 	| 00 	| 01 	| 11 	| 10 	|
|---	|---	|---	|---	|---	|
| 00 	| 1 	| 0 	| x 	| 1 	|
| 01 	| 0 	| 1 	| x 	| 1 	|
| 11 	| 1 	| 1 	| x 	| x 	|
| 10 	| 1 	| 1 	| x 	| x 	|

Sa = i3 + i2i0 + (i2i0)´ + i1 

## 5. Simulación del funcionamiento del circuito 
Los subsistemas del funcionamiento completo del circuito fueron probados medidante un teste bench que abarca desde el algoritmo de Hamming hasta el display de los 7 segmentos. 
### Codificador de entrada (7,4):

Data=0000 -> Enc=0000000 Parity=0  

Data=1101 -> Enc=1100110 Parity=0  

Data=1111 -> Enc=1111111 Parity=1  

### Caso 1: Double error detection:

Recibido=11111111 -> Corregido=1111, OneError=0, TwoError=1  

### Caso 2: Single error detection:

Recibido=11111110 -> Corregido=0111, OneError=1, TwoError=0  

### Ejemplo: 1010

Sin errores: 

Recibido=11010010 -> Decodificado=1010, Err1=0, Err2=1

Un error: 

Recibido=11010110 -> Decodificado=1011, Err1=1, Err2=0

Dos errores: 

Recibido=11011110 -> Decodificado=1011, Err1=0, Err2=1

Tabla de resultados: 
| Caso      | Entrada  | Salida | OneError | TwoError |
|-----------|----------|--------|----------|----------|
| 0 errores | 11010010 | 1010   | 0        | 1        |
| 1 error   | 11010110 | 1011   | 1        | 0        |
| 2 errores | 11111100 | 1011   | 0        | 1        |

## 6. Consumo de recursos 
Según lo desplegado en el programa de system verilog en cuánto a utilización de los recursos, estos corresponden a un 0.31%, es decir se implementan 27/8640 células. Su velocidad máxima es de 26.84 MHz y su consumo de potencia estático es de 3mW. Por lo tanto, el circuito implementado es eficiente y de bajo consumo energético. 

![Consumo de recursos](images/consumo.png) 

*Figura 2: Consumo de recursos*

## 7. Problemas y resoluciones 
Inicialemente, los problemas consistieron en la conexión del circuito como tal, ya que los pines colocados no correspondían a los pines programados, por lo que se tuvo que montar y desmontar en diversidad de ocasiones. Posteriormente, el problema consistió en que, a pesar de que el test bench corría correctamente, el display no mostraba el número correcto correspondiente a la posición del error, sino que, siempre mostraba la misma cifra. Por úlitmo, existe un "desfase" de 1 en el síndrome del test bench, ya que, este despliega una cifra después de la posición del error en la palabra, es decir, si el error está en la posición 4 este despliega el 5. 

# Parte 2: Oscilador de anillo 
## Descripción general
La segunda parte del proyecto consistió en armar un oscilador de anillo para determinar experimentalmente el tiempo de propagación (t_pd) de los inversores TTL 74LS04 mediante la medición del periodo. 
## Equipo utilizado: 

-Hex inversor TTL 74LS04

-Fuente de alimentación 

-Osciloscopio digital

-Cables y protoboard

## Procedimiento: 
El circuito se armó tomando en cuenta la estructura interna de la compuerta como se muestra en el siguiente diagrama: 

![Estructura interna del Hex inversor TTL 74LS04](images/inversor.png) 

*Figura 3: Estructura interna del Hex Inversor TTL 74LS04*

La fuente de alimentación se configuró a +5V, en este caso se conectó al pin 14 y GND al pin número 7. Para la configuración de los inversores en serie, se siguió el diagrama de manera que tanto para el caso de 3 inversores, como el de 5, las conexiones fueran correctas. Se conectaron ambos canales del osciloscopio para poder observar tanto la salida como la entrada y se configuró la escala de manera adecuada para poder visualizar un periodo completo. Las mediciones fueron realizadas mediante los cursos que posee el osciloscopio, se obtuvieron los siguientes resultados para el oscilador de 3 inversores: 

**Periodo:** T = 4,224ns

**Voltaje máximo:** V_max = 328mV

**Volatje mínimo:** V_min = -472mV 

### Cálculo del retardo de propagación:

tₚₓ = T / (2 × n) = 4.224 ns / (2 × 3) = 0.704 ns

Donde:
- tₚₓ: Retardo de propagación promedio
- T: Periodo medido (4.224 ns) 
- n: Número de inversores (3)

Además, se adjunta el periodo obtenido en las imágenes posteriores: 

![Periodo oscilador de 3 inversores](images/osciloscopio1.png)

*Figura 4: Oscilador de anillo con 3 inversores*

![Periodo oscilador de 5 inversores](images/osciloscopio2.png) 

*Figura 5: Oscilador de anillo con 3 inversores*

Para el oscilador de 5 inversores se puede verificar que: 

**Periodo:** T = 7,04ns

**Frecuencia:** 142 MHz

T_teórico = 2 × n × t_PD
          = 2 × 5 × 0.704 ns
          = 7.04 ns
## Análisis de resultados 
Los resultados obtenidos del tiempo de propagación corresponden a 0,70ns, lo cual es menor de lo esperado para un inversor de este tipo (9-15ns), esto se puede deber a diversas cargas como lo son una mala configuración o bien capacitancias parásitas. 
## Bitácora 

### Bitácora Jiahui Zhong Zhe 

![Bitácora Jiahui 1](images/jiahui1.png) 

![Bitácora Jiahui 2](images/jiahui2.png) 

![Bitácora Jiahui 3](images/jiahui3.png) 

![Bitácora Jiahui 4](images/jiahui4.png) 

![Bitácora Jiahui 5](images/jiahui5.png) 

![Bitácora Jiahui 6](images/jiahui6.png) 

![Bitácora Jiahui 7](images/jiahui7.png) 

![Bitácora Jiahui 8](images/jiahui8.png) 

### Bitácora Ximena Araya Brenes

![Bitácora Ximena 1](images/ximena1.png) 

![Bitácora Ximena 2](images/ximena2.png) 

![Bitácora Ximena 3](images/ximena3.png) 

![Bitácora Ximena 4](images/ximena4.png) 

![Bitácora Ximena 5](images/ximena5.png) 

![Bitácora Ximena 6](images/ximena6.png) 

![Bitácora Ximena 7](images/ximena7.png) 

![Bitácora Ximena 8](images/ximena8.png) 

![Bitácora Ximena 9](images/ximena9.png) 
