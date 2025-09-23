# Informe Proyecto corto I: Diseño digital combinacional en dispositivos programables 
## Jiahui Zhong Zhe 
## Ximena Araya Brenes 
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
<center>
  **Figura 1**: Diagrama de bloques 
![Diagrama de bloques](Escritorio/diagrama de bloques.png)
*Figura 1: Interconexión de subsistemas del corrector de errores*

</center>



