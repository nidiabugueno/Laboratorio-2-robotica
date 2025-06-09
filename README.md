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
