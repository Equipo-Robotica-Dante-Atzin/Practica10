# Práctica 10

## Integrantes

- Dante Mejía Silva
- Atzin Morales Alejandre

## Introducción

En esta práctica de laboratorio, se desarrolló un proyecto interdisciplinario que combinó procesamiento digital de imágenes con programación de robots industriales. El objetivo fue transformar una imagen en escala de grises en coordenadas específicas que pudieran ser interpretadas por un robot Epson C4 para recrear la figura en un plano físico.

Se comenzó seleccionando una imagen y procesándola mediante MATLAB para convertirla a blanco y negro utilizando técnicas adaptativas de binarización. Este procesamiento permitió simplificar la imagen a puntos relevantes que el robot podría interpretar. Las coordenadas de los píxeles en blanco se extrajeron y almacenaron en un archivo de texto que serviría como entrada para el robot.

Posteriormente, en el entorno de programación Epson RC+, se utilizó el archivo generado para que el robot interpretara las coordenadas y ejecutara movimientos precisos sobre un área de trabajo. De esta forma, el robot replicó el contorno de la imagen en una superficie utilizando herramientas y movimientos específicos, demostrando la capacidad de integrar software de visión con sistemas de control robótico.

## Instrucciones

En primer lugar se desarrolló el código en MATLAB necesario para la práctica.
```
clc
clear all
close all

color = imread('paloma-2.jpg');
imshow(color);

gris = rgb2gray(color);
figure
imshow(gris)

blancoynegro = imbinarize(gris, 'adaptive', 'Sensitivity', 0.68);
figure
blancoynegro = not(blancoynegro)
imshow(blancoynegro)

blancoynegro = sparse(blancoynegro)
```
### Código en MATLAB: Procesamiento de la Imagen

El código en MATLAB tuvo como finalidad procesar una imagen para extraer coordenadas relevantes que pudieran ser interpretadas por un robot. 

Primero, se cargó la imagen original y se convirtió a escala de grises, lo que permitió simplificar la información al trabajar solo con niveles de intensidad luminosa. Posteriormente, se aplicó una binarización adaptativa para segmentar la imagen en blanco y negro, destacando las áreas de interés. Luego, los colores se invirtieron para resaltar las zonas que el robot debía interpretar como trazos.

Finalmente, la imagen binarizada se convirtió en una matriz dispersa. Esto permitió almacenar solo las posiciones de los píxeles blancos, reduciendo significativamente el tamaño de los datos y facilitando la exportación de las coordenadas necesarias para el control del robot.

El resultado del código desarrollado fue el siguiente:

![image](https://github.com/user-attachments/assets/77e30637-e597-419c-93a7-2da7c6d16f84)

Las coordenadas extraídas de la imagen se almacenaron en un archivo de texto con un formato específico, diseñado para ser interpretado directamente por el código desarrollado en el software del robot Epson. Este archivo sirvió como puente entre el procesamiento en MATLAB y la ejecución robótica, permitiendo que el robot reconociera y utilizara las coordenadas para reproducir la imagen físicamente al correr el programa.
```
Function main
	Reset
	Motor On
	Power High
	Speed 40
	Accel 40, 40
	SpeedS 600
	AccelS 6000, 6000
	
	String linea$, xt$, yt$
	Integer x, y, largo, archivo, espacio, i
	
	Boolean fin
	fin = 1
	i = 1
	
	archivo = FreeFile
	ROpen "datos.txt" As #archivo
	
	Do While fin
		Input #archivo, linea$
		largo = Len(linea$)
		espacio = InStr(linea$, " ")
		yt$ = Mid$(linea$, 1, espacio - 1)
		xt$ = Mid$(linea$, espacio + 1, largo - espacio)
		x = Val(xt$)
		y = Val(yt$)
		
		Print "columna = ", x, "renglon = ", y
		Go inicio +X(x * 2) -Y(y * 2)
		
		i = i + 1
		
		Move Here -Z(20)
		Move Here +Z(20)
		
		fin = Eof(archivo) + 1
	
	Loop
	
	Power Low
Fend
```
En el software Epson RC+, el código tiene como objetivo interpretar las coordenadas generadas previamente y controlar al robot para recrear físicamente una imagen. 

Primero, se inicializa el robot configurando parámetros básicos como velocidad, aceleración y potencia. Luego, se abre un archivo de texto que contiene las coordenadas en formato X, Y. El programa lee cada línea del archivo, separa las coordenadas y las convierte a valores numéricos.

Para cada conjunto de coordenadas, el robot se mueve horizontalmente a la posición especificada y realiza un movimiento vertical simulado para marcar un punto. Este ciclo se repite hasta que se procesan todas las líneas del archivo. Al final, el programa apaga el motor y concluye. Este código permite que el robot trace patrones o contornos según las coordenadas proporcionadas.

Nuestra selección de imagen fue la siguiente:

![paloma-2](https://github.com/user-attachments/assets/d307c11d-2c77-434f-9a61-c24b92ced574)

Luego de ejecutar el código en el brazo robótico el resultado final fue el siguiente:

![Imagen de WhatsApp 2024-11-19 a las 16 49 05_fbfb64c4](https://github.com/user-attachments/assets/cbf65c16-6dcc-4352-8276-6caf2f5ad61c)

## Conclusiones

***Atzin Morales Alejandre:*** 



***Dante Mejía Silva:*** 
La práctica realizada evidenció la capacidad de integrar el procesamiento digital de imágenes con el control de sistemas robóticos, un componente esencial en aplicaciones de automatización avanzada. Utilizando MATLAB, se procesó una imagen y se extrajeron las coordenadas correspondientes para representar su estructura de manera binaria. Estas coordenadas fueron luego utilizadas por el robot Epson C4 a través de un código desarrollado en el software Epson RC+, permitiendo al robot replicar físicamente el patrón de la imagen mediante movimientos precisos.

Este ejercicio subraya la importancia de la interconexión entre herramientas de visión por computadora y tecnologías de control robótico en la automatización moderna. La correcta extracción y representación de datos, así como la programación detallada del robot, son fundamentales para asegurar que el sistema realice tareas complejas de manera eficiente y precisa. La práctica no solo refuerza el entendimiento de cómo los robots pueden interactuar con el entorno de manera autónoma, sino que también resalta el papel crucial que juegan las metodologías de procesamiento de imágenes en la mejora de la precisión y funcionalidad de los sistemas automatizados.

## Referencias Bibliográficas 

[1] 	EpsonCompany, «Especialistas en automatización industrial». 2024, https://www.epson.es/es_ES/robots


