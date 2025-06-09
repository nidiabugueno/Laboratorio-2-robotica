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
