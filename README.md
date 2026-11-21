# Orange Pi – Ethernet 16 Relay Control
### Web-Based Remote Control System for 16 GPIO Relays

**Ethernet 16 Relay Control** is a web-based hardware control project developed by **Vratislav Režo (VRDRAGON)**.

The project uses an Orange Pi single-board computer to control up to 16 relay outputs through a local Ethernet network. A PHP-based web interface allows users to remotely switch individual GPIO outputs, monitor their states and manage system settings.

The application combines PHP, JavaScript, MySQL and Python to create an integrated hardware control platform.

---

## Project Overview

The main objective of this project is to build a remotely accessible relay control system using an Orange Pi board.

The system provides a browser-based interface where each relay can be independently switched ON or OFF. The current relay states are represented visually using colored indicators.

The application also includes an administration interface for configuring network parameters, user credentials and relay labels.

### Main Features

- Control of 16 independent relay outputs.
- Remote GPIO switching through a web browser.
- Visual indication of relay states (ON/OFF).
- AJAX-based communication between JavaScript and PHP.
- User login and session management.
- Administration panel.
- Configuration of Ethernet IP address.
- Modification of login credentials.
- Customization of individual relay button names.
- Remote system reboot functionality.
- MySQL database integration.
- Python scripts for network configuration and system initialization.

---

## System Architecture

The project uses several technologies working together:

| Component | Technology | Purpose |
|---|---|---|
| Hardware | Orange Pi | Main control unit |
| GPIO | WiringOP-Zero | GPIO pin management |
| Backend | PHP | Web application and hardware control |
| Frontend | HTML / CSS | User interface |
| Communication | JavaScript / AJAX | Asynchronous relay control |
| Database | MySQL | Users, IP configuration and relay labels |
| System scripts | Python | Network configuration and GPIO initialization |
| Web server | Apache | Hosting the application |

---

## How It Works

### 1. Web Interface

The main application (`index.php`) generates a web interface containing 16 relay controls.

Each relay is represented by an individual button with a visual state indicator.

The interface reads the GPIO states and displays the corresponding indicator:

- Red – relay output LOW.
- Green – relay output HIGH.

### 2. GPIO Control

The application uses WiringOP-Zero to communicate with the Orange Pi GPIO interface.

The GPIO configuration maps 16 logical relay positions to physical WiringOP pin identifiers.

| Relay | WiringOP pin |
|---|---:|
| 1 | 14 |
| 2 | 11 |
| 3 | 10 |
| 4 | 6 |
| 5 | 5 |
| 6 | 4 |
| 7 | 1 |
| 8 | 15 |
| 9 | 8 |
| 10 | 9 |
| 11 | 7 |
| 12 | 0 |
| 13 | 2 |
| 14 | 3 |
| 15 | 12 |
| 16 | 13 |

### 3. Asynchronous Communication

The JavaScript implementation (`script.js`) uses XMLHttpRequest to communicate with the PHP GPIO handler.

When a user clicks a relay indicator:

1. JavaScript sends an HTTP GET request to `gpio.php`.
2. The PHP script identifies the requested relay.
3. The current GPIO state is read.
4. The output state is inverted.
5. The new state is written to the GPIO pin.
6. The updated state is returned to the browser.
7. The visual indicator is updated.

This allows relay switching without reloading the entire web page.

### 4. User Authentication

The project includes a basic PHP login mechanism.

Authenticated users can access the administration interface, where they can manage system settings.

The authentication implementation uses PHP sessions and a MySQL users table.

### 5. Administration Panel

The administration interface (`welcome.php`) provides several configuration options:

- Change username and password.
- Change Ethernet IP address.
- Change relay button names.
- Reboot the Orange Pi system.
- Sign out of the administration interface.

### 6. Network Configuration

Python scripts provide additional system functionality.

`test.py` retrieves the configured IP address from MySQL and applies it to the Ethernet interface using `ifconfig`.

`restart.py` contains an implementation concept for restoring network and user settings using a GPIO input.

---

## Database Structure

The project uses a MySQL database named `login`.

The original installation instructions define three main tables.

### Users

Stores user authentication information.

| Column | Description |
|---|---|
| id | User identifier |
| usernme | Username |
| password | Password |

### IP

Stores the configured Ethernet IP address.

| Column | Description |
|---|---|
| id | Configuration identifier |
| IP_adrr | IP address |

### Button

Stores custom names for relay controls.

| Column | Description |
|---|---|
| id | Relay identifier |
| name | Custom relay label |

---

## Project Structure

```text
php-login/
│
├── index.php          # Main relay interface and login
├── welcome.php        # Administration panel
├── session.php        # Session validation
├── logout.php         # User logout
│
├── gpio.php            # GPIO relay switching
├── config.php          # MySQL configuration
│
├── chip.php            # Ethernet IP configuration
├── chup.php            # Username and password configuration
├── chbt.php            # Relay button name configuration
│
├── script.js           # AJAX communication and UI updates
├── background.jpg      # Interface background
│
├── data/
│   └── img/
│       ├── green/      # Relay ON indicators
│       └── red/        # Relay OFF indicators
│
├── install.sh          # Installation script
├── install.txt         # Installation and database instructions
├── null.sh             # GPIO initialization
│
├── test.py             # Network configuration
└── restart.py          # GPIO-triggered configuration concept
```

---

## Hardware and Software Requirements

### Hardware

- Orange Pi single-board computer compatible with the original GPIO configuration.
- 16-channel relay module.
- Ethernet network connection.
- Suitable power supply for the Orange Pi and relay module.
- Connected electrical loads (optional).

### Software

The original project installation instructions reference:

- Armbian Linux.
- Apache HTTP Server.
- PHP 5.
- MySQL Server.
- Python 2.
- Python MySQLdb library.
- WiringOP-Zero GPIO library.

**Note:** These are historical software dependencies. The original implementation uses legacy PHP and Python interfaces and is not directly compatible with many modern Linux distributions and PHP versions.

---

## Installation

The repository includes an original installation script (`install.sh`) and additional instructions (`install.txt`).

The original installation process was designed around an Armbian-based Orange Pi environment.

The general setup involves:

1. Installing and configuring Armbian.
2. Installing Apache and PHP.
3. Installing MySQL.
4. Installing WiringOP-Zero.
5. Configuring the MySQL database.
6. Deploying the PHP application into the web server directory.
7. Configuring the GPIO pins.
8. Setting up startup scripts.

Refer to `install.txt` for the original installation commands and database configuration.

**Important:** The original installation scripts contain outdated package names and commands, and should not be executed directly on a modern system without reviewing and adapting them.

---

## Security Considerations

This repository represents an early experimental hardware control implementation.

The original source code uses legacy authentication and database practices. In particular:

- Passwords are stored and compared in plaintext.
- Database credentials are embedded in `config.php`.
- The GPIO endpoint does not implement authentication checks.
- Relay switching uses HTTP GET requests.
- The original application does not implement CSRF protection.
- The main interface exposes GPIO control independently of the administration login.

The application should therefore be treated as a historical prototype and should not be exposed to the public internet or used to control safety-critical electrical equipment without substantial security and hardware modifications.

---

## Project Purpose and Historical Context

This project demonstrates the integration of embedded hardware, web technologies and database management into a remotely accessible control system.

It combines several engineering disciplines:

- Embedded Linux.
- GPIO hardware control.
- Web application development.
- Client-server communication.
- Database management.
- Network configuration.
- Hardware and software integration.

The project illustrates an early practical implementation of network-connected hardware control using a single-board computer.

---

## Author

**Vratislav Režo (VRDRAGON)**

GitHub: [@VRDragon](https://github.com/VRDragon)

Repository: [php-login](https://github.com/VRDragon/php-login)

---

*An experimental web-based 16-channel GPIO relay control system developed using Orange Pi, PHP, JavaScript, MySQL and Python.*
