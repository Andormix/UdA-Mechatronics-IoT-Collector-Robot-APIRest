# Colector de Cubos - Servidor Web & Controlador IoT

<p align="center">
  <img width="100%" alt="image" src="https://github.com/user-attachments/assets/8fbf1ce1-9692-4903-89ff-d6cf3e931d8a" />
</p>


Aquest projecte implementa un servidor web basat en **Flask** i **Python** dissenyat per executar-se en una **Raspberry Pi**. Actua com a node central de recepció de dades sensorials (humitat, temperatura, CO₂, TVOC i sensors d'obstacles / final de carrera), enregistra la informació en una base de dades **MySQL** i controla actuadors físics (LEDs / releus) mitjançant els pins **GPIO**.

---

## Estructura del Codi

```plaintext
.
├── app.py              # Servidor principal Flask, rutes API, connexió DB i control GPIO
└── README.md           # Documentació del projecte
```

# Documentació Tècnica del Sistema

## Funcionalitats Principals

* **Recepció de Telemetria (`/dades`):** Endpoint `GET` per a la lectura i processament de dades ambientals i d'estat de switches.
* **Persistència de Dades:** Inserció automàtica en temps real de les meures de temperatura, humitat, CO₂ i TVOC a la base de dades MySQL (`juanreptem3`).
* **Lògica d'Alarma i Actuació (GPIO):**
  * **Alarma Ambiental:** Si la humitat és superior al 25% o la temperatura supera els 20°C, s'activa el LED/Releu d'alarma (`GPIO 17`).
  * **Detecció d'Obstacles:** Si el sensor `Switch1` s'activa (`1`), s'encén el LED d'avís d'obstacle (`GPIO 11`).
  * **Detecció de Final de Carrera:** Lectura del sensor `Switch2` per a la notificació d'arribada al final del recorregut (`GPIO 26`).
* **Control Manual de LEDs (`/led`):** Permet encendre o apagar el LED d'alarma de manera remota mitjançant paràmetres de consulta (`?led=on` / `?led=off`).

---

## Assignació de Pins GPIO (Raspberry Pi)

| Component / Funció | Pin GPIO (`gpiozero`) | Descripció |
| :--- | :--- | :--- |
| **LED Alarma Vermell** | `GPIO 17` | S'activa per alta temperatura o humitat excessiva. |
| **LED Avís Obstacle** | `GPIO 11` | S'activa quan el sensor d'obstacles detecta presència (`Switch1`). |
| **LED Avís Final** | `GPIO 26` | Reservat per a la detecció de final de línia/recorregut (`Switch2`). |

---

## Esquema de la Base de Dades

El servidor es connecta a una base de dades MySQL en la xarxa local:

* **Host:** `192.168.3.10`
* **Base de dades:** `juanreptem3`
* **Taula:** `Dades`
* **Camps inserits:** `(DadesID, Humetat, Temperatura, CO2, TVOC)`

---

## Endpoints de l'API REST

### 1. Estat del Servidor
* **Ruta:** `/`
* **Mètode:** `GET`
* **Descripció:** Comprova si el servidor web està actiu.

### 2. Recepció de Telemetria
* **Ruta:** `/dades`
* **Mètode:** `GET`
* **Paràmetres URL:**
  * `Temperatura`: Valor float (°C)
  * `Humetat`: Valor float (%)
  * `CO2`: Concentració de CO₂ (ppm)
  * `tvoc`: Nivell de TVOC (ppb)
  * `Switch1`: Estat del sensor d'obstacles (`1` o `0`)
  * `Switch2`: Estat del sensor de final de carrera (`1` o `0`)

**Exemple de petició:**
```http
GET [http://192.168.13.238:8080/dades?Temperatura=22&Humetat=26&CO2=400&tvoc=0&Switch1=1&Switch2=0](http://192.168.13.238:8080/dades?Temperatura=22&Humetat=26&CO2=400&tvoc=0&Switch1=1&Switch2=0)
