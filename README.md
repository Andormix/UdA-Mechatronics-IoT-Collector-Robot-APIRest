# IoT Collector Robot — REST API & Embedded Control

[![Arduino](https://img.shields.io/badge/Arduino-C%2B%2B-00979D?style=flat&logo=arduino&logoColor=white)](#)
[![VEX Robotics](https://img.shields.io/badge/VEX-Robotics-ED1C24?style=flat)](#)
[![Python](https://img.shields.io/badge/Python-Flask-3776AB?style=flat&logo=python&logoColor=white)](#)
[![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-IoT-C51A4A?style=flat&logo=raspberrypi&logoColor=white)](#)
[![MySQL](https://img.shields.io/badge/Database-MySQL-4479A1?style=flat&logo=mysql&logoColor=white)](#)
[![Universitat d'Andorra](https://img.shields.io/badge/Academic-Universitat%20d'Andorra-003366?style=flat)](#)

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/8fbf1ce1-9692-4903-89ff-d6cf3e931d8a"
    alt="IoT Collector Robot"
    width="100%"
  />
</p>

An embedded IoT collector robot developed with C++, Arduino-compatible hardware, VEX Robotics components, a NodeMCU Wi-Fi module, and a Raspberry Pi backend.

The system combines autonomous and programmed robot movement, environmental sensor acquisition, REST API communication, remote status monitoring, GPIO-based alerts, and MySQL data persistence.

The project was developed as part of the academic work at the **Universitat d'Andorra (UdA)**.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Descripción del Proyecto](#descripción-del-proyecto)
- [System Architecture](#system-architecture)
- [Main Features](#main-features)
- [Repository Structure](#repository-structure)
- [Hardware Components](#hardware-components)
- [Embedded Firmware](#embedded-firmware)
- [Robot Control](#robot-control)
- [Environmental Monitoring](#environmental-monitoring)
- [REST API Server](#rest-api-server)
- [API Endpoints](#api-endpoints)
- [GPIO Assignment](#gpio-assignment)
- [Database](#database)
- [Communication Flow](#communication-flow)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the System](#running-the-system)
- [Example Requests](#example-requests)
- [Safety and Security Notes](#safety-and-security-notes)
- [Troubleshooting](#troubleshooting)
- [Future Improvements](#future-improvements)
- [Academic Context](#academic-context)
- [License](#license)

---

## Project Overview

This project implements a connected collector robot capable of moving through a predefined route, collecting environmental data, detecting obstacles and end-of-route conditions, and sending telemetry to a Raspberry Pi through a Wi-Fi network.

The Raspberry Pi runs a Flask web server that:

1. Receives sensor data through HTTP GET requests.
2. Processes temperature, humidity, CO₂, and TVOC values.
3. Detects obstacle and end-of-route states.
4. Activates physical LEDs or relays through GPIO.
5. Stores telemetry in a MySQL database.
6. Provides remote control of the alarm LED.

---

## Descripción del Proyecto

El proyecto consiste en un robot recolector conectado a una red Wi-Fi, capaz de desplazarse por un recorrido predefinido, recoger datos ambientales y transmitirlos a un servidor central.

El sistema utiliza diferentes componentes electrónicos y plataformas:

- Un controlador basado en C++ para la lógica de movimiento.
- Un módulo NodeMCU para la conexión Wi-Fi.
- Sensores de temperatura, humedad, CO₂ y TVOC.
- Una Raspberry Pi como servidor IoT.
- Una API REST desarrollada con Flask.
- Una base de datos MySQL para almacenar la telemetría.
- LEDs o relés conectados a los GPIO de la Raspberry Pi para indicar alarmas y eventos.

---

## System Architecture

```text
┌────────────────────────────┐
│      Robot Controller      │
│        VEX / C++           │
│                            │
│  - Drivetrain              │
│  - Distance sensor         │
│  - Vision sensor           │
│  - Collection mechanism    │
└──────────────┬─────────────┘
               │
               │ Sensor states
               │
┌──────────────▼─────────────┐
│       NodeMCU / Wi-Fi       │
│                             │
│  - DHT temperature/humidity │
│  - CCS811 CO₂ / TVOC        │
│  - Switch 1                 │
│  - Switch 2                 │
│  - HTTP GET client          │
└──────────────┬──────────────┘
               │
               │ Wi-Fi / REST API
               │
┌──────────────▼─────────────┐
│       Raspberry Pi          │
│       Flask Server           │
│                             │
│  - Receives telemetry       │
│  - Controls GPIO outputs    │
│  - Generates alerts         │
│  - Stores measurements      │
└──────────────┬──────────────┘
               │
               │ MySQL connection
               │
┌──────────────▼─────────────┐
│        MySQL Database        │
│                             │
│  - Temperature              │
│  - Humidity                 │
│  - CO₂                      │
│  - TVOC                     │
│  - Measurement timestamp    │
└─────────────────────────────┘
```

---

## Main Features

- Programmed robot navigation using VEX C++.
- Configurable route composed of straight sections and turns.
- Distance-based movement control.
- Vision and distance sensor integration.
- Wireless communication through NodeMCU.
- Environmental monitoring using DHT and CCS811 sensors.
- REST API communication over HTTP.
- Raspberry Pi Flask server.
- MySQL telemetry persistence.
- Obstacle detection.
- End-of-route detection.
- Environmental alarm logic.
- Remote LED control through HTTP requests.
- GPIO-based event notification.

---

## Repository Structure

```text
.
├── main.cpp
├── nodeMCU_WiFi_Raspi_Final.ino
├── ServidorFinal.py
└── README.md
```

### File Descriptions

| File | Description |
| :--- | :--- |
| `main.cpp` | VEX C++ program for robot navigation, drivetrain control, distance sensing, and route execution |
| `nodeMCU_WiFi_Raspi_Final.ino` | NodeMCU firmware for sensor acquisition and Wi-Fi communication with the Raspberry Pi |
| `ServidorFinal.py` | Flask REST API server, GPIO controller, alarm logic, and MySQL persistence |
| `README.md` | Project documentation |

---

## Hardware Components

### Robot Controller

The robot control program is designed for a VEX-based platform and uses configured devices such as:

| Device | Type | Port or Configuration |
| :--- | :--- | :--- |
| Drivetrain | VEX drivetrain | Ports `17`, `18`, and `1` |
| Distance Sensor | VEX distance sensor | Port `11` |
| Collection Motor | VEX motor | Port `20` |
| Vision Sensor | VEX vision sensor | Port `9` |

> The exact port assignments depend on the VEX configuration generated by the project environment.

### IoT Sensor Node

| Component | Function |
| :--- | :--- |
| NodeMCU | Wi-Fi connectivity and sensor data transmission |
| DHT sensor | Temperature and humidity measurement |
| CCS811 | CO₂ and TVOC measurement |
| Switch 1 | Obstacle or object detection |
| Switch 2 | End-of-route or limit-switch detection |

### Raspberry Pi Server

| Component | Function |
| :--- | :--- |
| Raspberry Pi | Local IoT server and GPIO controller |
| GPIO 17 | Environmental alarm LED or relay |
| GPIO 11 | Obstacle warning LED |
| GPIO 26 | End-of-route indicator |
| MySQL server | Telemetry storage |

---

## Embedded Firmware

### VEX C++ Controller

The `main.cpp` file contains the robot movement logic. It defines route parameters such as:

```cpp
const int offset = 260;

float rectas[] = {
    2000 + offset
};

bool curvas[] = {};
```

The route is represented using:

- Straight-line distances.
- Turn directions.
- Initial position compensation.
- Motor and sensor control functions.

The robot can be configured to follow a sequence of straight sections and 90-degree turns.

### NodeMCU Firmware

The `nodeMCU_WiFi_Raspi_Final.ino` sketch is responsible for:

- Connecting to the configured Wi-Fi network.
- Reading temperature and humidity.
- Reading CO₂ concentration.
- Reading TVOC concentration.
- Reading obstacle and end-of-route switches.
- Building the HTTP request.
- Sending data to the Raspberry Pi REST server.

The transmitted query string follows this structure:

```text
?Temperatura=<value>
&Humetat=<value>
&CO2=<value>
&tvoc=<value>
&Switch1=<value>
&Switch2=<value>
```

---

## Robot Control

The movement program uses a VEX drivetrain and route arrays to define the robot's path.

### Route Parameters

| Parameter | Description |
| :--- | :--- |
| `offset` | Compensation applied to the initial robot position |
| `rectas[]` | Array containing distances for straight sections |
| `curvas[]` | Array containing turn directions |

Turn values are represented as:

```text
true  = right turn
false = left turn
```

The route can be modified by updating the distance and turn arrays in `main.cpp`.

### Distance and Vision Sensors

The robot includes:

- A distance sensor for detecting nearby objects.
- A vision sensor for object or environment recognition.
- A motor dedicated to the collection mechanism.

These sensors can be used to improve navigation, payload handling, and obstacle detection.

---

## Environmental Monitoring

The NodeMCU collects data from two main sensor systems.

### DHT Sensor

The DHT sensor provides:

- Temperature in degrees Celsius.
- Relative humidity in percentage.

Example variables:

```cpp
float hum;
float temp;
```

### CCS811 Sensor

The CCS811 sensor provides:

- Equivalent CO₂ concentration in parts per million.
- Total volatile organic compounds in parts per billion.

Example variables:

```cpp
uint16_t eco2;
uint16_t etvoc;
uint16_t errstat;
uint16_t raw;
```

The data is accepted only when the CCS811 status indicates a valid reading.

---

## REST API Server

The Raspberry Pi server is implemented in Python using Flask.

The main server file is:

```text
ServidorFinal.py
```

The server is responsible for:

- Starting the Flask application.
- Determining or configuring the Raspberry Pi IP address.
- Receiving data from the NodeMCU.
- Writing measurements to MySQL.
- Controlling GPIO LEDs.
- Generating environmental warnings.
- Reporting obstacle detection.
- Reporting end-of-route detection.
- Providing remote alarm LED control.

### Basic Server Response

The root endpoint returns a message confirming that the web server is running:

```text
Rebuda una petició, Servidor web actiu
```

---

## API Endpoints

### 1. Server Status

Checks whether the Flask server is active.

| Property | Value |
| :--- | :--- |
| Route | `/` |
| Method | `GET` |
| Description | Returns a server status message |

#### Example

```http
GET http://<RASPBERRY_PI_IP>:8080/
```

#### Expected Response

```text
Rebuda una petició, Servidor web actiu
```

---

### 2. Telemetry Reception

Receives environmental and robot status information from the NodeMCU.

| Property | Value |
| :--- | :--- |
| Route | `/dades` |
| Method | `GET` |
| Description | Receives telemetry and switch states |

#### Query Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `Temperatura` | Float | Temperature in degrees Celsius |
| `Humetat` | Float | Relative humidity percentage |
| `CO2` | Integer or Float | CO₂ concentration in ppm |
| `tvoc` | Integer or Float | TVOC concentration in ppb |
| `Switch1` | `0` or `1` | Obstacle sensor state |
| `Switch2` | `0` or `1` | End-of-route sensor state |

#### Example Request

```http
GET http://<RASPBERRY_PI_IP>:8080/dades?Temperatura=22&Humetat=26&CO2=400&tvoc=0&Switch1=1&Switch2=0
```

#### Example Using cURL

```bash
curl "http://<RASPBERRY_PI_IP>:8080/dades?Temperatura=22&Humetat=26&CO2=400&tvoc=0&Switch1=1&Switch2=0"
```

---

### 3. Alarm LED Control

Controls the environmental alarm LED remotely.

| Property | Value |
| :--- | :--- |
| Route | `/led` |
| Method | `GET` |
| Description | Turns the alarm LED on or off |

#### Query Parameter

| Parameter | Value | Description |
| :--- | :--- | :--- |
| `led` | `on` | Turns the LED on |
| `led` | `off` | Turns the LED off |

#### Turn the LED On

```http
GET http://<RASPBERRY_PI_IP>:8080/led?led=on
```

```bash
curl "http://<RASPBERRY_PI_IP>:8080/led?led=on"
```

#### Turn the LED Off

```http
GET http://<RASPBERRY_PI_IP>:8080/led?led=off
```

```bash
curl "http://<RASPBERRY_PI_IP>:8080/led?led=off"
```

---

## Alarm and Event Logic

### Environmental Alarm

The server activates the environmental alarm LED when either of the following conditions is met:

```text
Humidity > 25%
Temperature > 20°C
```

The corresponding alarm output is connected to GPIO 17.

### Obstacle Detection

When `Switch1` is equal to `1`, the server detects an obstacle and activates the obstacle warning output.

```text
Switch1 == "1"
```

The corresponding output is connected to GPIO 11.

### End-of-Route Detection

When `Switch2` is equal to `1`, the server detects that the robot has reached the end of the route.

```text
Switch2 == "1"
```

The corresponding output is reserved for GPIO 26.

---

## GPIO Assignment

The Raspberry Pi GPIO outputs are controlled through the `gpiozero` Python library.

| Component or Function | GPIO Pin | Description |
| :--- | :---: | :--- |
| Environmental alarm LED or relay | `GPIO 17` | Activated by high temperature or excessive humidity |
| Obstacle warning LED | `GPIO 11` | Activated when `Switch1` detects an obstacle |
| End-of-route indicator | `GPIO 26` | Reserved for the `Switch2` end-of-route event |

> GPIO numbering refers to the BCM numbering scheme used by `gpiozero`.

---

## Database

The Flask server stores telemetry in a MySQL database.

### Database Structure

| Property | Value |
| :--- | :--- |
| Database engine | MySQL |
| Table | `Dades` |
| Primary or identifier field | `DadesID` |
| Humidity field | `Humetat` |
| Temperature field | `Temperatura` |
| CO₂ field | `CO2` |
| TVOC field | `TVOC` |

### Inserted Data

The server inserts the following values:

```text
DadesID
Humetat
Temperatura
CO2
TVOC
```

The identifier is generated from the current time using the format:

```text
HHMMSS
```

### Example SQL Operation

The server uses an insertion query equivalent to:

```sql
INSERT INTO Dades
    (DadesID, Humetat, Temperatura, CO2, TVOC)
VALUES
    (%s, %s, %s, %s, %s);
```

> Database credentials, host addresses, and passwords should be configured securely and should not be committed to a public repository.

---

## Communication Flow

```text
1. The robot moves through the configured route.
2. The NodeMCU reads temperature and humidity from the DHT sensor.
3. The NodeMCU reads CO₂ and TVOC values from the CCS811 sensor.
4. The NodeMCU reads the obstacle and end-of-route switches.
5. The NodeMCU builds an HTTP GET request.
6. The request is sent through Wi-Fi to the Raspberry Pi.
7. Flask receives the request at `/dades`.
8. The server converts and processes the received values.
9. The telemetry is inserted into MySQL.
10. GPIO outputs are updated according to alarm conditions.
11. The server returns a status response to the NodeMCU.
```

---

## Requirements

### Embedded Hardware

- VEX-compatible robot controller.
- VEX drivetrain and motors.
- VEX distance sensor.
- VEX vision sensor.
- NodeMCU or compatible ESP8266 board.
- DHT temperature and humidity sensor.
- CCS811 air-quality sensor.
- Obstacle switch or limit switch.
- End-of-route switch.
- Wi-Fi network.
- Raspberry Pi.
- LEDs or relays.
- MySQL server.

### Software

- VEXcode or compatible VEX C++ development environment.
- Arduino IDE.
- ESP8266 board support for Arduino IDE.
- Python 3.
- Flask.
- `gpiozero`.
- `mysql-connector-python`.
- MySQL Server.
- Compatible sensor libraries:
  - DHT sensor library.
  - DHT sensor type library.
  - CCS811 sensor library.

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Andormix/UdA-mecatronica-robot-recolector-APIRest-IoT.git
cd UdA-mecatronica-robot-recolector-APIRest-IoT
```

### 2. Install Python Dependencies

On the Raspberry Pi, install the required packages:

```bash
sudo apt update
sudo apt install python3 python3-pip
```

Install the Python libraries:

```bash
pip3 install flask gpiozero mysql-connector-python
```

Depending on the Raspberry Pi operating system, the packages may also be installed using the system package manager.

### 3. Install Arduino Dependencies

Open `nodeMCU_WiFi_Raspi_Final.ino` in the Arduino IDE and install the libraries required by the sketch.

Typical dependencies include:

- DHT sensor library.
- DHT sensor type library.
- CCS811 library.
- ESP8266 Wi-Fi support.

### 4. Configure the NodeMCU Board

In the Arduino IDE:

1. Select the appropriate ESP8266 or NodeMCU board.
2. Select the correct serial port.
3. Configure the Wi-Fi credentials.
4. Configure the Raspberry Pi server IP address.
5. Verify the sensor wiring.
6. Upload the sketch.

### 5. Configure the Raspberry Pi

Before starting the server:

1. Connect the Raspberry Pi to the same network as the NodeMCU.
2. Configure the MySQL connection.
3. Verify that the database and `Dades` table exist.
4. Connect the LEDs or relays to the configured GPIO pins.
5. Start the Flask server.

---

## Configuration

### Wi-Fi Configuration

The NodeMCU sketch contains the Wi-Fi configuration:

```cpp
const char* ssid = "YOUR_WIFI_NETWORK";
const char* password = "YOUR_WIFI_PASSWORD";
```

Configure the server address using the Raspberry Pi IP:

```cpp
const String host = "RASPBERRY_PI_IP";
```

For example:

```cpp
const String host = "192.168.1.100";
```

### Flask Server Configuration

The Flask server listens on port `8080`:

```python
port = 8080
```

For local-network access, the server should bind to an address reachable by the NodeMCU.

### MySQL Configuration

Use environment variables or a separate configuration file instead of storing credentials directly in the source code.

Example environment variables:

```bash
export MYSQL_HOST="192.168.1.10"
export MYSQL_DATABASE="robot_database"
export MYSQL_USER="robot_user"
export MYSQL_PASSWORD="change_this_password"
```

The MySQL database and table should be created before starting the telemetry server.

---

## Running the System

### Start the Flask Server

From the Raspberry Pi:

```bash
python3 ServidorFinal.py
```

The server should listen on:

```text
http://<RASPBERRY_PI_IP>:8080
```

### Test the Server

Check the root endpoint:

```bash
curl "http://<RASPBERRY_PI_IP>:8080/"
```

Test telemetry reception:

```bash
curl "http://<RASPBERRY_PI_IP>:8080/dades?Temperatura=22&Humetat=26&CO2=400&tvoc=0&Switch1=0&Switch2=0"
```

Test LED control:

```bash
curl "http://<RASPBERRY_PI_IP>:8080/led?led=on"
curl "http://<RASPBERRY_PI_IP>:8080/led?led=off"
```

### Start the Robot

1. Power on the robot controller.
2. Verify the drivetrain and sensor connections.
3. Power on the NodeMCU.
4. Confirm that it connects to Wi-Fi.
5. Start the Raspberry Pi Flask server.
6. Confirm that telemetry requests arrive at `/dades`.
7. Test the robot route and collection mechanism.
8. Monitor the GPIO indicators and database entries.

---

## Example End-to-End Request

The NodeMCU sends a request similar to:

```http
GET /dades?Temperatura=22.5&Humetat=26.0&CO2=400&tvoc=12&Switch1=0&Switch2=0 HTTP/1.1
Host: 192.168.1.100:8080
```

The Raspberry Pi then:

1. Parses the query parameters.
2. Converts numeric values to the appropriate types.
3. Stores the values in MySQL.
4. Checks the environmental thresholds.
5. Updates the GPIO outputs.
6. Returns a status message.

---

## Safety and Security Notes

- Do not expose the Flask server directly to the public internet without authentication.
- Do not commit Wi-Fi passwords to the repository.
- Do not commit MySQL passwords or private IP credentials.
- Use environment variables or a local configuration file for secrets.
- Validate and sanitize all values received through HTTP requests.
- Add authentication before using the `/led` endpoint in an untrusted network.
- Use HTTPS or a secure tunnel when communicating outside the local network.
- Check the voltage and current requirements of all LEDs, relays, motors, and sensors.
- Use transistor or relay driver circuits when GPIO pins cannot directly drive an actuator.
- Disconnect power before changing the robot's wiring.
- Verify that the robot has an emergency stop procedure.

---

## Troubleshooting

### The NodeMCU Does Not Connect to Wi-Fi

Check:

- SSID and password.
- Wi-Fi frequency compatibility.
- Raspberry Pi and NodeMCU network connection.
- Server IP address.
- Signal strength.
- Power supply stability.

### The Raspberry Pi Does Not Receive Data

Check:

- The Flask server is running.
- Port `8080` is available.
- The Raspberry Pi and NodeMCU are on the same network.
- The configured host IP is correct.
- The request URL contains all required parameters.
- The firewall is not blocking port `8080`.

### MySQL Connection Fails

Check:

- MySQL service status.
- Database name.
- User permissions.
- Server IP address.
- Network connectivity.
- Credentials.
- Existence of the `Dades` table.

### Sensor Values Are Invalid

Check:

- Sensor wiring.
- Sensor power supply.
- Correct Arduino libraries.
- DHT sensor type configuration.
- CCS811 initialization.
- I²C connections for the CCS811.
- Sensor warm-up time.

### GPIO Indicators Do Not Work

Check:

- BCM GPIO numbering.
- LED polarity.
- Current-limiting resistors.
- Relay driver circuitry.
- Raspberry Pi permissions.
- GPIO pin conflicts.

### The Robot Does Not Follow the Route

Check:

- VEX device configuration.
- Drivetrain motor ports.
- Distance sensor port.
- Vision sensor configuration.
- Values in `rectas[]`.
- Values in `curvas[]`.
- Initial position offset.
- Battery charge.
- Mechanical alignment of the wheels.

---

## Future Improvements

Potential improvements include:

- Replace HTTP GET telemetry with POST requests.
- Add authentication to the REST API.
- Add HTTPS support.
- Validate all API parameters.
- Use JSON payloads instead of query parameters.
- Add automatic retry logic for failed requests.
- Add local data buffering when Wi-Fi is unavailable.
- Add a web dashboard for live telemetry.
- Add route configuration through the API.
- Add remote robot start and stop controls.
- Add payload detection and verification.
- Add battery-level monitoring.
- Add data visualization from the MySQL database.
- Move secrets to environment variables.
- Add automated tests for the Flask endpoints.
- Add structured logging.
- Add Docker support for the server.
- Add watchdog and automatic service restart.
- Add improved obstacle avoidance.
- Add a dedicated emergency stop mechanism.

---

## Academic Context

This project combines concepts from:

- Embedded systems.
- C++ programming.
- Python programming.
- Robotics.
- Internet of Things.
- REST API design.
- Wireless communication.
- Sensor acquisition.
- GPIO control.
- Database persistence.
- Motor control.
- Autonomous navigation.
- Hardware-software integration.

The project was developed as part of the academic work at the **Universitat d'Andorra (UdA)**.

---

## License

This project is intended for academic, educational, and experimental purposes.

Unless otherwise specified, the source code and documentation are provided for learning and non-commercial use. Contact the repository owner before redistributing the project or using it in a commercial product.

---

## Author

Developed by **Andormix** for the **Universitat d'Andorra**.

Repository:

```text
https://github.com/Andormix/UdA-mecatronica-robot-recolector-APIRest-IoT
```
