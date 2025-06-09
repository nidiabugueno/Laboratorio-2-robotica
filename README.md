# Laboratorio-2-robotica
Laboratorio 2 Robótica

Tema: Sensores, Percepción y Planificación con procesamiento de datos en Robótica Móvil

Integrantes:

1. Luciano Alonso Cubillos Bugueño

2. Vicente Jose Montiel Torres

3. Sebastián Maximiliano Jeria López

4. Javiera Paz Cabrera Cácerez

5. Nidia Antonella Bugueño Rodríguez


## Solución del laboratorio 2

# Parte 1:
- Conectar los sensores ultrasónicos HC-SR04 y RGB en Arduino.
  En esta sección se muestra la conexión individual de los sensores que serán utilizados más adelante en el proyecto.
Primero, en la siguiente imagen se puede ver el sensor ultrasónico HC-SR04 conectado al Arduino. Este sensor se utiliza para medir distancias mediante pulsos de sonido, y es útil para detectar obstáculos frente al robot.
![Imagen 1](images/WhatsApp%20Image%202025-06-08%20at%2022.45.59.jpeg)

Luego, en la siguiente imagen se muestra el sensor de color RGB TCS34725, también conectado al Arduino. Este sensor permite detectar colores predominantes (rojo, verde, azul, blanco o negro) en cualquier superficie circundante al robot (segun la direccion en la que se encuentre orientado el sensor).
![Imagen 2](images/WhatsApp%20Image%202025-06-08%20at%2022.47.06.jpeg)
Ambos sensores han sido conectados y probados por separado, pero más adelante serán integrados de manera conjunta para que el robot pueda tomar decisiones combinando información de distancia y color, como parte de su sistema de percepción y navegación.

- Programar Arduino para leer la distancia con HC-SR04 y mostrarla en el monitor serie.
**Código:**
```
const int trigPin = 9; 
const int echoPin = 10; 

void setup() {
pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT);
Serial.begin(9600); 
}

void loop() {
// Pulso ultrasónico
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

long duration = pulseIn(echoPin, HIGH);
float distance = duration * 0.034 / 2;

Serial.print("Distancia: ");
Serial.print(distance);
Serial.println(" cm");

delay(500);
}

```
En el video se puede observar la implementación del código, donde el sensor ultrasónico HC-SR04, acompañado de una cinta métrica como referencia, mide la distancia a un objeto colocado frente a él. Los valores obtenidos se muestran en centímetros y presentan un nivel de precisión razonable, respaldado por la comparación con una medición real realizada con la cinta métrica. 
**Vídeo:** https://drive.google.com/file/d/1vQFNfrGTwaHnhcnHTr9eiN4DzGNuPJnD/view

- Programar Arduino para leer los valores RGB y mostrar el color detectado.
**Código:**
```
#include <Wire.h>
#include "Adafruit_TCS34725.h"

// Crear objeto del sensor
Adafruit_TCS34725 tcs = Adafruit_TCS34725(
  TCS34725_INTEGRATIONTIME_700MS, TCS34725_GAIN_1X);

void setup() {
  Serial.begin(9600);
  
  if (tcs.begin()) {
    Serial.println("Sensor TCS34725 detectado.");
  } else {
    Serial.println("No se encontró el sensor TCS34725.");
    while (1); // Se detiene
  }
}

void loop() {
  uint16_t r, g, b, c;
  
  tcs.getRawData(&r, &g, &b, &c);
  
  // Calcula el valor de luminosidad y color corregido
  uint16_t colorTemp = tcs.calculateColorTemperature(r, g, b);
  uint16_t lux = tcs.calculateLux(r, g, b);

  Serial.print("Rojo: "); Serial.print(r);
  Serial.print(" Verde: "); Serial.print(g);
  Serial.print(" Azul: "); Serial.print(b);
  Serial.print(" Clear: "); Serial.print(c);
  Serial.print(" Lux: "); Serial.print(lux);
  Serial.print(" Temp Color: "); Serial.print(colorTemp);
  Serial.println(" K");

  delay(1000);
}

```
En el siguiente video se muestra la implementación del código, donde se probaron distintos objetos con colores y superficies variadas. Gracias al sensor RGB TCS34725, fue posible detectar la predominancia de uno de los colores estándar (rojo, verde, azul, blanco o negro) en valores RGB. Esta información permitirá, más adelante, determinar con mayor precisión el color del objeto en base al aumento de los valores correspondientes en cada componente RGB.    
**Vídeo:** https://drive.google.com/file/d/1NyO_SfWbi5NMau5B_5BJvJpCkFXGl8nn/view

- Analizar la precisión de los sensores en diferentes condiciones (luz, superficie, distancia).
**Código:**
```
//Se utilizan los mismos códigos de los apartados anteriores (Programar Arduino para leer la distancia con HC-SR04 y mostrarla en el monitor serie y Programar Arduino para leer los valores RGB y mostrar el color detectado)
```
En los siguientes videos se presentan dos escenarios de medición del sensor RGB TCS34725: el primero con luz ambiental → https://drive.google.com/file/d/1CmzlompqTkbdZA1xP7yJ5ihHcB2ezfuI/view y el segundo con baja iluminación → https://drive.google.com/file/d/1SMbk8u2Qp7k1s9sjKIr5A0dqsPjTusrS/view .

En ambos casos se utilizaron los mismos objetos, aunque con algunas diferencias en las propiedades de sus superficies. A pesar de estas variaciones, el sensor RGB TCS34725 logró detectar correctamente los colores. Esta consistencia en los resultados se debe a que el sensor incorpora su propia fuente de luz (luz LED blanca), lo que le permite funcionar de manera confiable tanto en condiciones de buena iluminación como en ambientes oscuros. 
Además, en el apartado correspondiente a “Programar Arduino para leer los valores RGB y mostrar el color detectado”, también se observa un desempeño similar al del primer caso, confirmando la eficacia del sensor bajo distintas condiciones de luz.

El otro sensor estudiado, HC-SR04, que mide la proximidad ultrasónica, se probó en condiciones con luz ambiental y en entornos de poca luz en el video https://drive.google.com/file/d/1vQFNfrGTwaHnhcnHTr9eiN4DzGNuPJnD/view .

El comportamiento del sensor al funcionar en estos ambientes es estable y no se ven cambios significativos en su medición al ser un sensor que trabaja de manera ultrasónica. 


# Preguntas parte 1:
**- ¿Qué es la percepción en robótica y por qué es fundamental en los sistemas autónomos?**

**Respuesta:** La percepción en robótica es la capacidad que tiene un robot para obtener información mediante sensores, como cámaras, ultrasonido, infrarrojos, entre otros. La información entregada por estos componentes es útil, ya que permiten que el robot pueda tomar decisiones en base a los datos recolectados y los pueda transformar en información valiosa para navegar, interactuar, detectar objetos/obstáculos, seguir caminos, además de otro tipos de acciones que si son integradas de forma correcta permiten un comportamiento autónomo en estos sistemas.

**- En el sensor ultrasónico HC-SR04 ¿Qué parámetro se mide para calcular la distancia?**

**Respuesta:** El sensor HC-SR04 utiliza una señal ultrasónica para calcular la distancia, el ultrasonido mide el tiempo que tarda la señal emitida (ultrasónica) en ir hasta un objeto y volver, por lo general se compone de los siguientes pasos:

1. **Emisión del pulso:** El microcontrolador envía una señal de activación de 10 µs al pin TRIG. El sensor emite una onda ultrasónica de 40 kHz.
2. **Rebote del pulso:** La onda se propaga por el aire, choca con un objeto y regresa como eco al sensor. 
3. **Recepción del eco:** El sensor activa el pin ECHO durante el tiempo en que tarda la onda en ir y volver
4. **Cálculo de distancia:** El tiempo total de viaje del sonido se convierte en distancia usando una fórmula.
   
	La fórmula utilizada para medir la distancia es la siguiente: **Distancia = (V * T)/2** 
**V** = velocidad del sonido en el aire (aproximadamente = 343 m/s)

**T** = Tiempo de ida y vuelta que tarda el sonido en llegar al sensor (medido en segundos → s)

**- ¿Cómo influye el ruido en las mediciones del sensor ultrasónico y cómo podría reducirse?**

Respuesta: El ruido puede provocar que el sensor pueda recolectar lecturas inestables o incorrectas, las cuales pueden ser provocadas por:

-Superficies irregulares.

-Interferencias acústicas (otros sonidos).

-Una gran cantidad de reflexiones o reflexiones múltiples.

Para poder hacer frente e intentar reducir el ruido que pueden captar estos sensores, se pueden considerar las siguientes estrategias:

-Utilización de filtros (media móvil, media ponderada, pasa bajos)

-Calibración del sensor

-Filtro de kalman que nos da ventajas como las siguientes:

	- Considera la incertidumbre (ruido) de cada fuente. 
 
	- Es recursivo, no necesita almacenar todos los datos anteriores. 
 
	- Se adapta dinámicamente a los cambios en el entorno.
 
Otra opción sería la fusión de sensores, pero en este caso no contamos con más.
# Análisis de mejoras generales
Al haber incorporado el uso de ambos sensores por separado, se ha evidenciado su aplicación al mundo real, comportamiento, funcionamiento y  qué factores pueden influir en sus mediciones. 
Como grupo creemos que la incorporación de estos sensores a robots favorecen a su percepción del entorno que los rodea, sin embargo, hay que tener en cuenta el ruido y las condiciones en las que esté sumergido el robot. Aunque encontramos que los sensores utilizados tienen un comportamiento coherente al posicionarse en distintas condiciones, no pudimos determinar si un tipo de ruido o errores sistemáticos afectaron la medición realizada. 
Creemos que una revisión más exhaustiva de los sensores RGB TCS34725 y HC-SR04 ante distintas superficies y distancias más grandes podría beneficiar el estudio. Además, si hubiésemos utilizado un valor “pivote”, sería más adecuada la medición, ya que podríamos comprobar errores de offset y de escala.

# Parte 2: Cinemática y Dinámica de Robots Móviles usando un IMU
- Aplicar umbralización al sensor ultrasónico para detectar si hay un obstáculo a menos de 10 cm.
**Código:**
```
const int trigPin = 9;
const int echoPin = 10;

long duration;
float distanceCm;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() {
  // Enviar pulso ultrasónico
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // Leer duración del eco
  duration = pulseIn(echoPin, HIGH);
  
  // Calcular distancia en cm
  distanceCm = (duration * 0.0343) / 2;
  
  // Mostrar distancia
  Serial.print("Distancia: ");
  Serial.print(distanceCm);
  Serial.print(" cm - ");

  // Aplicar umbral a 10 cm
  if (distanceCm <= 10 && distanceCm > 0) {
    Serial.println("¡Obstáculo detectado!");
  } else {
    Serial.println("Área libre");
  }
  
  delay(500); // Esperar medio segundo antes de la siguiente lectura
}

```
El siguiente video muestra la implementación del código, donde se utiliza el sensor ultrasónico HC-SR04 junto a una cinta métrica como referencia. El sistema permite detectar si un objeto se encuentra a una distancia menor o igual a 10 cm, e imprime el resultado en pantalla, indicando si hay un obstáculo presente o si el área está libre.

Para lograr esto, el sensor envía un pulso ultrasónico y mide el tiempo que tarda en recibir el eco reflejado por un objeto. A partir de este tiempo, se calcula la distancia utilizando la velocidad del sonido. Luego, se aplica una umbralización simple, comparando la distancia medida con un valor fijo (10 cm).

Si la distancia está dentro de este rango, el sistema interpreta que hay un obstáculo cercano y lo reporta por el monitor serial. Esta lógica básica permite sentar las bases para futuras decisiones de navegación autónoma, como detenerse, girar o evitar el objeto detectado. Además, la cinta métrica se utiliza en el video como herramienta de validación para comprobar que las mediciones del sensor son consistentes y precisas. 
**Vídeo:** https://drive.google.com/file/d/1fgnj1V5RRpBAysteTnsBfT1R620tS3kC/view
**Código:**
```
#include <Wire.h>
#include "Adafruit_TCS34725.h"

// Inicializa el sensor (ajusta según tu sensor)
Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_50MS, TCS34725_GAIN_4X);

void setup() {
  Serial.begin(9600);
  if (tcs.begin()) {
    Serial.println("Sensor de color iniciado");
  } else {
    Serial.println("No se encontró el sensor de color");
    while (1);
  }
}

void loop() {
  uint16_t r, g, b, c;
  tcs.getRawData(&r, &g, &b, &c);

  // Evitar división por cero
  if (c == 0) c = 1;

  // Calcular valores normalizados
  float fr = (float)r / c;
  float fg = (float)g / c;
  float fb = (float)b / c;

  // Mostrar valores normalizados (opcional)
  Serial.print("fr: "); Serial.print(fr, 3);
  Serial.print(" fg: "); Serial.print(fg, 3);
  Serial.print(" fb: "); Serial.println(fb, 3);

  // Clasificación de color basada en valores normalizados
  if (c <30) {  // umbral para considerar "negro"
    Serial.println("Negro");
  } 
  else if (fr > 0.3 && fg > 0.35 && fb > 0.25) {
    Serial.println("Blanco");
  }
  else if (fr > 0.5 && fg < 0.35 && fb < 0.3) {
    Serial.println("Rojo");
  } 
  else if (fr < 0.4 && fg > 0.45 && fb < 0.3) {
    Serial.println("Verde");
  } 
  else if (fr < 0.3 && fg < 0.4 && fb > 0.45) {
    Serial.println("Azul");
  } 
  else {
    Serial.println("Color desconocido");
  }
  delay(1000);  // Espera 1 segundo para la siguiente lectura
}

```
En el siguiente video se muestra la implementación del código que permite detectar colores utilizando el sensor RGB TCS34725. Para ello, se definieron umbrales de referencia que permiten identificar los colores rojo, verde, azul, blanco y negro, en base a los valores normalizados de las componentes R, G y B.

Durante la prueba, se utilizaron cinco objetos con superficies de colores definidos (rojo, verde, azul, negro y blanco). El sensor logró identificar correctamente el color predominante en cada uno de ellos, imprimiendo en pantalla el resultado correspondiente.

Esto es posible gracias al proceso de normalización, que ajusta los valores de rojo, verde y azul en relación a la intensidad total de luz captada, permitiendo una mejor comparación entre colores sin importar las condiciones de iluminación. Además, se utiliza un umbral bajo para reconocer el color negro (muy baja luz reflejada) y combinaciones altas en las tres componentes para identificar el blanco.

Este método simple pero efectivo demuestra cómo, a través de la comparación de los valores normalizados con umbrales predefinidos, el sistema puede clasificar colores de manera confiable.
**Vídeo:** https://drive.google.com/file/d/1UIbyVI3_FJRf7YfKGJmKwZzWPEKoUTET/view

- Implementar un algoritmo en Arduino que detenga el robot ante obstáculos y cambie de dirección según el color detectado.
```

```
- Probar navegación en un circuito con obstáculos y superficies en diferentes colores.
```

```
- Ajustar parámetros para mejorar la detección y estabilidad del sistema.
```

```
- Implementación de estrategias de navegación basadas en reglas.
```

```

# Preguntas parte 2:
**-Si el robot detecta el color rojo en el suelo ¿Qué acción debería tomar? ¿Por qué?**
Respuesta: Si el robot detecta el color rojo en el suelo, debería girar a la derecha. Esto se debe a que en el código se ha implementado una función llamada “girarDerecha()”, la cual se ejecuta específicamente cuando se identifica el color rojo como predominante.

Esta acción puede tener distintos propósitos según el diseño lo que se tenga pensado para un circuito, por ejemplo:

-Evitar zonas restringidas o peligrosas que están marcadas con color rojo.

-Seguir una ruta predefinida, donde el rojo actúa como una señal para cambiar de dirección.

En este caso, el color rojo se utiliza como una señal de giro que permite al robot ajustar su trayectoria y continuar su recorrido de forma autónoma según las reglas definidas en el programa.

**-Si el sensor ultrasónico detecta valores erráticos ¿Qué estrategias podemos aplicar para mejorar la precisión?**

Respuesta: Si el sensor ultrasónico presenta valores erráticos, se pueden aplicar diversas estrategias para mejorar la precisión de las mediciones:

-Filtrado de datos: Una de las soluciones más comunes es aplicar filtros como la media móvil, media ponderada o filtros pasa bajos. Estos permiten reducir el ruido en las lecturas y obtener resultados más estables.

-Calibración: Otra estrategia es la calibración del sensor, que consiste en ajustar sus mediciones comparándolas con valores de referencia reales, como una regla o cinta métrica, para corregir posibles desviaciones.

-Fusión de sensores: En sistemas más complejos, se puede aplicar fusión de sensores (por ejemplo, combinando datos de un IMU o cámara), lo cual permite obtener información más confiable. Sin embargo, en este caso no contamos con sensores adicionales, por lo que esta técnica no es aplicable.

Estas estrategias ayudan a que el robot tome decisiones más precisas y seguras, especialmente en entornos donde puede haber interferencias o superficies irregulares

**-Si tuvieras que integrar un nuevo sensor para mejorar la navegación del robot ¿Cuál elegirías y por qué?**

Respuesta: Una opción sería integrar un sensor LIDAR o una cámara (RGB o RGB-D) con una librería centrada en robótica (por ejemplo, OpenCV). Estos sensores nos podrían dar las siguientes ventajas:

-Proporcionar un mapeo más preciso del entorno ya sea 2D o 3D.

-Mejora en la planificación de rutas al poder detectar distintos elementos del entorno como líneas, objetos, señales o realizar seguimiento visual.




