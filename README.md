int verde = 5;
int amarillo = 6;
int rojo = 7;
int pulsador = 9;

int valor = 0;
int valor_antiguo = 0;
int duracionRojo = 5000;     // 5 segundos
int duracionAmarillo = 3000; // 3 segundos
int duracionVerde = 6000;    // 6 segundos
int ajuste = 3000;           // Ajuste de tiempo para la luz verde
unsigned long tiempoInicioVerde = 0; // Para controlar el tiempo del verde
bool pulsado = false;        // Variable para indicar si se ha pulsado el botón

void setup() {
  pinMode(verde, OUTPUT);
  pinMode(amarillo, OUTPUT);
  pinMode(rojo, OUTPUT);
  pinMode(pulsador, INPUT);
}

void loop() {
  // Ciclo del semáforo
  // Luz roja
  digitalWrite(rojo, HIGH);
  delay(duracionRojo);
  digitalWrite(rojo, LOW);

  // Luz amarilla
  digitalWrite(amarillo, HIGH);
  delay(duracionAmarillo);
  digitalWrite(amarillo, LOW);

  // Luz verde
  digitalWrite(verde, HIGH);
  tiempoInicioVerde = millis(); // Guardamos el tiempo de inicio del verde
  unsigned long duracionActualVerde = duracionVerde; // Duración actual del verde

  // Mientras el verde esté encendido, comprobamos el pulsador
  while (millis() - tiempoInicioVerde < duracionActualVerde) {
    valor = digitalRead(pulsador);

    // Si el botón es presionado y aún no se ha reducido el tiempo
    if ((valor == HIGH) && (valor_antiguo == LOW) && !pulsado) {
      duracionActualVerde = max(duracionActualVerde - ajuste, 1000); // Reducimos 3 segundos
      pulsado = true; // Indicamos que ya se hizo la reducción
    }

    valor_antiguo = valor;

    // Continuamos el ciclo si no ha pasado todo el tiempo
    if (millis() - tiempoInicioVerde >= duracionActualVerde) {
      break;
    }
  }

  // Apagar la luz verde y resetear el estado del botón
  digitalWrite(verde, LOW);
  pulsado = false;
}
