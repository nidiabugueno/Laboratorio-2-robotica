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

# Parte 1: Configuración del Hardware y pruebas iniciales 
- Conectar los sensores ultrasónicos HC-SR04 y RGB en Arduino.
  En esta sección se muestra la conexión individual de los sensores que serán utilizados más adelante en el proyecto.
Primero, en la siguiente imagen se puede ver el sensor ultrasónico HC-SR04 conectado al Arduino. Este sensor se utiliza para medir distancias mediante pulsos de sonido, y es útil para detectar obstáculos frente al robot.
![Imagen 1](images/WhatsApp%20Image%202025-06-08%20at%2022.45.59.jpeg)

Luego, en la siguiente imagen se muestra el sensor de color RGB TCS34725, también conectado al Arduino. Este sensor permite detectar colores predominantes (rojo, verde, azul, blanco o negro) mediante valores RGB en cualquier superficie circundante al robot (segun la direccion en la que se encuentre orientado el sensor), integrando una luz LED blanca para facilitar su detección.
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

**-Definir umbrales para detectar colores, rojo, verde y azul usando el sensor RGB**
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

Específicamente se utilizaron umbrales definidos como fr, fg y fb (rojo, verde y azul respectivamente)  además de c como la luminosidad de la siguiente manera:

Rojo: fr > 0.5; fg < 0.35; fb < 0.3

Verde:  fr < 0.4; fg > 0.45; fb < 0.3

Azul:  fr < 0.3; fg < 0.4; fb > 0.45

Negro: c < 30 (baja exposición de luz)

Blanco: fr > 0.3; fg > 0.35; fb > 0.25 (todos los colores balanceados)


Este método simple pero efectivo demuestra cómo, a través de la comparación de los valores normalizados con umbrales predefinidos, el sistema puede clasificar colores de manera confiable.
**Vídeo:** https://drive.google.com/file/d/1UIbyVI3_FJRf7YfKGJmKwZzWPEKoUTET/view

- Implementar un algoritmo en Arduino que detenga el robot ante obstáculos y cambie de dirección según el color detectado.

**Código:**
```
// Pines para motor izquierdo
const int IN1 = 8; 
const int IN2 = 9;
const int ENA = 10;

// Pines para motor derecho
const int IN3 = 6;
const int IN4 = 7;
const int ENB = 5;

// Pines sensor ultrasónico
const int trigPin = 2;
const int echoPin = 3;

// Librerías
#include <Wire.h>
#include "Adafruit_TCS34725.h"

// Crear instancia del sensor de color
Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_50MS, TCS34725_GAIN_4X);

void setup() {
  Serial.begin(9600);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  // Sensor TCS
  if (tcs.begin()) {
    Serial.println("Sensor TCS34725 detectado");
  } else {
    Serial.println("No se encontró sensor TCS34725");
    while (1);
  }
}

void avanzar() {
  analogWrite(ENA, 180);
  analogWrite(ENB, 180);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void retroceder() {
  analogWrite(ENA, 120);
  analogWrite(ENB, 120);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void girarIzquierda() {
  analogWrite(ENA, 180);
  analogWrite(ENB, 180);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void girarDerecha() {
  analogWrite(ENA, 220);
  analogWrite(ENB, 220);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void detener() {
  analogWrite(ENA, 0);
  analogWrite(ENB, 0);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

float leerDistancia() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  long duracion = pulseIn(echoPin, HIGH);
  float distancia = (duracion * 0.0343) / 2;
  return distancia;
}

void loop() {
  float distCentro = leerDistancia();

  if (distCentro <= 5 && distCentro > 0) {
    detener();
    delay(1000);
    // Leer color
    uint16_t r, g, b, c;
    tcs.getRawData(&r, &g, &b, &c);
    if (c == 0) c = 1;
    float fr = (float)r / c;
    float fg = (float)g / c;
    float fb = (float)b / c;

    Serial.print("fr: "); Serial.print(fr, 3);
    Serial.print(" fg: "); Serial.print(fg, 3);
    Serial.print(" fb: "); Serial.println(fb, 3);

    if (c < 50) {
      Serial.println("Negro. Retrocediendo.");
      avanzar();
      delay(2000);
    } else if (fr > 0.3 && fg > 0.35 && fb > 0.25) {
      Serial.println("Blanco. Avanzando.");
      retroceder();
      delay(1000);
    } else if (fr > 0.5 && fg < 0.35 && fb < 0.3) {
      Serial.println("Rojo. Girando a la derecha.");
      girarDerecha();
      delay(1000);
      avanzar();
      delay(3000);
    } else if (fr < 0.4 && fg > 0.45 && fb < 0.3) {
      Serial.println("Verde. Girando a la izquierda.");
      girarIzquierda();
      delay(1000);
      avanzar();
      delay(1000);
    } else if (fr < 0.3 && fg < 0.4 && fb > 0.45) {
      Serial.println("Azul. Retrocediendo.");
      girarIzquierda();
      delay(1000);
      avanzar();
      delay(1000);
    } else {
      Serial.println("Color desconocido. Retrocediendo.");
      retroceder();
      delay(1000);
    }

    detener(); // Detenerse después de realizar la acción
  }

  delay(1000); // Espera antes de volver a medir
}


```

En el siguiente video se observa la implementación de un algoritmo que integra la detección de obstáculos mediante un sensor ultrasónico (HC-SR04) y la identificación de colores del suelo utilizando el sensor RGB TCS34725. El comportamiento del robot fue programado para realizar distintas acciones dependiendo del color detectado, lo que permite simular un recorrido autónomo guiado por estímulos visuales.
El circuito parte con el robot sobre una superficie de color negro. Al detectar este color, el robot avanza hasta llegar al siguiente punto. Cuando el sensor RGB identifica el color verde, el robot ejecuta un giro hacia la izquierda y continúa avanzando. Posteriormente, al llegar al color rojo, realiza una vuelta completa (giro a la derecha) y vuelve a avanzar hasta llegar al color azul. En este caso, gira nuevamente a la izquierda y sigue su recorrido hasta encontrar el color blanco, donde finalmente realiza una maniobra de retroceso.
Este comportamiento es posible gracias a una lógica condicional que actúa cuando el sensor ultrasónico detecta un obstáculo a una distancia igual o menor a 5 cm. En ese momento, el robot se detiene, toma una lectura de color mediante el sensor RGB y ejecuta la acción correspondiente. Todo este proceso puede observarse claramente en el video, donde se valida el funcionamiento del algoritmo diseñado. Más adelante, este tipo de comportamiento será fundamental al integrar sensores y lógica de navegación en conjunto para tareas más complejas.
**Video:** https://drive.google.com/file/d/1xCKT389JeqGP5X-bCwkxL27pw-2nlUJG/view
- Probar navegación en un circuito con obstáculos y superficies en diferentes colores.
```
//Se utiliza el mismo código del apartado anterior (Implementar un algoritmo en Arduino que detenga el robot ante obstáculos y cambie de dirección según el color detectado.)
```
- Ajustar parámetros para mejorar la detección y estabilidad del sistema.

  Durante la implementación del sistema, fue necesario ajustar diversos parámetros para mejorar tanto la detección de colores como la respuesta del robot frente a obstáculos. Por ejemplo, se modificaron umbrales en el código para que el sensor RGB TCS34725 identificara con mayor precisión los colores rojo, verde, azul, blanco y negro. Estos umbrales se obtuvieron de forma empírica, observando los valores normalizados y ajustándolos hasta lograr una detección confiable en cada caso.
En cuanto al sensor ultrasónico HC-SR04, también se afinó el umbral de detección de obstáculos (5 cm), lo cual permitió una mejor sincronización entre la lectura de distancia y la posterior acción del robot.
Por otro lado, se observó que en algunos casos el robot cambiaba bruscamente de dirección o no seguía una trayectoria recta, lo que afectaba su estabilidad durante el recorrido. Esto podría haberse mejorado ajustando y calibrando los motores, ya que pequeñas diferencias en la velocidad de cada motor pueden hacer que el robot se desvíe con el tiempo. Un ajuste más fino en las señales PWM o una compensación entre motores habría permitido una trayectoria más estable y un comportamiento más preciso en cada maniobra.
Estos ajustes son parte esencial del proceso iterativo de mejora en sistemas robóticos, y serán fundamentales en futuras etapas del proyecto donde se requiera mayor precisión en la navegación y toma de decisiones.
```

```
- Implementación de estrategias de navegación basadas en reglas.
**Código:**
```
 if (c < 50) {
      Serial.println("Negro. Retrocediendo.");
      avanzar();
      delay(2000);
    } else if (fr > 0.3 && fg > 0.35 && fb > 0.25) {
      Serial.println("Blanco. Avanzando.");
      retroceder();
      delay(1000);
    } else if (fr > 0.5 && fg < 0.35 && fb < 0.3) {
      Serial.println("Rojo. Girando a la derecha.");
      girarDerecha();
      delay(1000);
      avanzar();
      delay(3000);
    } else if (fr < 0.4 && fg > 0.45 && fb < 0.3) {
      Serial.println("Verde. Girando a la izquierda.");
      girarIzquierda();
      delay(1000);
      avanzar();
      delay(1000);
    } else if (fr < 0.3 && fg < 0.4 && fb > 0.45) {
      Serial.println("Azul. Retrocediendo.");
      girarIzquierda();
      delay(1000);
      avanzar();
      delay(1000);
    } else {
      Serial.println("Color desconocido. Retrocediendo.");
      retroceder();
      delay(1000);
    }

    detener(); // Detenerse después de realizar la acción
  }

  delay(1000); // Espera antes de volver a medir
}


```
En este apartado se implementaron estrategias de navegación basadas en reglas condicionales, las cuales permiten que el robot tome decisiones específicas según el color detectado en el suelo mediante el sensor RGB TCS34725. Estas reglas están codificadas con una estructura if-else que evalúa el valor de los componentes rojo (fr), verde (fg), azul (fb) y la claridad total (c) para determinar a qué color corresponde la superficie detectada. En base a esto, se ejecuta una acción concreta.

Las decisiones del robot son las siguientes:

-Negro (c < 50): Cuando el sensor detecta poca luz reflejada (muy baja claridad), se interpreta como color negro. En este caso, el robot avanza durante 2 segundos, lo que simula que intenta escapar o corregir su trayectoria si detectó una zona oscura (por ejemplo, un borde o zona no deseada).

-Blanco (fr > 0.3 && fg > 0.35 && fb > 0.25): El blanco refleja todos los colores de forma bastante equilibrada, por lo que los valores normalizados son medios-altos en los tres componentes. Aquí, el robot retrocede brevemente, posiblemente como medida de corrección antes de buscar un nuevo camino.

-Rojo (fr > 0.5 && fg < 0.35 && fb < 0.3): El rojo se caracteriza por una alta componente en rojo y bajos en verde y azul. Al detectarlo, el robot gira a la derecha, espera un segundo y luego avanza durante 3 segundos, lo que representa una maniobra de giro seguida de desplazamiento.

-Verde (fr < 0.4 && fg > 0.45 && fb < 0.3): El verde se detecta cuando su componente es significativamente mayor que las otras. En este caso, el robot gira a la izquierda y luego avanza brevemente.

-Azul (fr < 0.3 && fg < 0.4 && fb > 0.45): Si el valor azul predomina, el robot interpreta que está sobre color azul. Aquí, el robot realiza un giro a la izquierda y luego avanza, como una forma de cambio de dirección.

-Color desconocido: Si los valores no encajan con ninguna de las condiciones anteriores, se considera que el color no es reconocible, por lo tanto, el robot retrocede como medida de seguridad o corrección.


Luego de ejecutar cada acción, el robot se detiene y espera un segundo antes de volver a analizar el entorno. Esta lógica de navegación basada en reglas es simple pero efectiva, ya que permite que el robot reaccione ante distintas situaciones del entorno de forma autónoma, sin necesidad de mapas ni planificación previa. Además, estas reglas pueden ser ajustadas fácilmente para adaptar el comportamiento a distintos escenarios.


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

**-¿Cuál es el tiempo de respuesta del robot al detectar un cambio de color?**

Respuesta: En este caso, se utiliza el sensor TCS34725, que destaca por su alta precisión en detección de color y por contar con un filtro IR incorporado que mejora la exactitud al eliminar interferencias del espectro infrarrojo. Además, este sensor incluye una fuente de luz LED blanca integrada, lo que le permite operar de forma confiable en diferentes condiciones de iluminación.

El tiempo de integración puede ser configurado en el código, esto significa que el sensor necesita al menos ese tiempo para captar y promediar la cantidad de luz recibida antes de entregar una lectura válida (por ejemplo, 50ms). Sin embargo, en la implementación práctica se agrega un delay(1000) (1 segundo) entre lecturas para asegurar estabilidad en las mediciones y facilitar la observación de resultados.

Por lo tanto, el tiempo de respuesta efectivo del robot es de aproximadamente 1 segundo, ya que es el intervalo con el que se está leyendo el sensor y ejecutando las decisiones asociadas (como girar o detenerse). Este valor puede optimizarse reduciendo el delay() o usando programación basada en temporizadores para permitir una reacción más inmediata del sistema.

#Análisis de mejoras generales:
Luego de haber profundizado en las aplicaciones del robot al utilizar ambos sensores estudiados, nos dimos cuenta de problemas que se pudieron observar al avanzar con la actividad. Uno de los problemas era el encontrarse con falsos positivos/negativos, por lo que el ajustar parámetros en la detección de obstáculos con el sensor ultrasónico HC-SR04 fue crucial para poder detectar los obstáculos de manera más ajustada a la inicial. Se espera que para próximas aplicaciones con estos sensores se tomen medidas como el promediado de las lecturas, suavizar las lecturas del sensor RGB TCS34725 con filtros como el tipo media móvil. Con estas mejoras se podría mejorar la robustez del sistema y hacerlo más confiable en otras condiciones de operación.



