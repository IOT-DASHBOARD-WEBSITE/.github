# 🌡️ IoT Dashboard - Smart Device Monitoring System

A comprehensive IoT dashboard system for real-time monitoring and control of ESP32-based sensors with Adafruit IO integration.

## 📋 Table of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Documentation](#api-documentation)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

### 🔧 Core Features
- **Real-time Sensor Monitoring**: Temperature, humidity, pressure, and light sensors
- **Device Management**: Register, configure, and control IoT devices
- **Live Dashboard**: Interactive charts and statistics
- **MQTT Integration**: Real-time data communication
- **Device Control**: WiFi configuration, data intervals, reboot commands

### 🔗 Adafruit IO Integration
- **Automatic Publishing**: Sensor data auto-published to Adafruit feeds
- **MQTT Subscription**: Receive device configuration updates
- **Real-time Sync**: Bidirectional communication between devices and cloud

### 📊 Dashboard Features
- **Interactive Charts**: Temperature/humidity trends using Recharts
- **Device Status**: Online/offline indicators
- **Statistics**: Min/max/average calculations
- **Responsive Design**: Mobile-friendly interface

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   ESP32 Device  │◄──►│   Adafruit IO   │◄──►│   Web Dashboard │
│                 │    │   MQTT/HTTP     │    │   Next.js       │
│ • Sensors       │    │ • Feeds         │    │ • Charts        │
│ • MQTT Client   │    │ • Real-time     │    │ • Controls      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────┐
                    │   NestJS API    │
                    │ • REST API      │
                    │ • WebSocket     │
                    │ • MongoDB       │
                    └─────────────────┘
```

## 🛠️ Tech Stack

### Backend
- **Framework**: NestJS (Node.js)
- **Database**: MongoDB with Mongoose
- **Real-time**: Socket.io, MQTT
- **Validation**: class-validator
- **Documentation**: Swagger (optional)

### Frontend
- **Framework**: Next.js 16 (React 19)
- **Styling**: Tailwind CSS + SCSS
- **Charts**: Recharts
- **State Management**: React Hooks
- **Real-time**: Socket.io Client

### IoT Integration
- **Platform**: Adafruit IO
- **Protocol**: MQTT + HTTP REST
- **Device**: ESP32 with sensors

### Deployment
- **Backend**: Render
- **Frontend**: Vercel
- **Database**: MongoDB Atlas

## 📋 Prerequisites

Before running this application, make sure you have:

### System Requirements
- **Node.js**: v18.17.0 or higher
- **npm**: v9.0.0 or higher (or yarn/pnpm)
- **MongoDB**: Local installation or MongoDB Atlas account

### Adafruit IO Setup
1. Create account at [Adafruit IO](https://io.adafruit.com)
2. Create the following feeds:
   - `yolofarm.farm-temperature`
   - `yolofarm.farm-humidity`
   - `yolofarm.farm-light-intensity`
   - `yolofarm.farm-wifi-ssid`
   - `yolofarm.farm-wifi-password`
   - `yolofarm.farm-send-interval`

### Hardware (Optional)
- ESP32 development board
- DHT11/DHT22 temperature & humidity sensor
- Optional: BMP180 pressure sensor, LDR light sensor

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone https://github.com/IOT-DASHBOARD-WEBSITE/your-repo-name.git
cd your-repo-name
```

### 2. Backend Setup
```bash
cd backend

# Install dependencies
npm install

# Copy environment file
cp .env.example .env
```

### 3. Frontend Setup
```bash
cd ../my-app

# Install dependencies
npm install

# Copy environment file
cp .env.example .env.local
```

## ⚙️ Configuration

### Backend Environment (.env)
```env
# Database
NODE_ENV=development
MONGODB_URI=mongodb://localhost:27017/iot_db
PORT=3001

# Frontend
FRONTEND_URL=http://localhost:3000

# Adafruit IO
ADAFRUIT_USERNAME=your_username
ADAFRUIT_KEY=your_aio_key
ADAFRUIT_USE_SSL=false

# Feed URLs (replace 'your_username' with actual username)
ADAFRUIT_TEMPERATURE_FEED=https://io.adafruit.com/api/v2/your_username/feeds/your_temp_feed
ADAFRUIT_HUMIDITY_FEED=https://io.adafruit.com/api/v2/your_username/feeds/your_humidity_feed
ADAFRUIT_LIGHT_FEED=https://io.adafruit.com/api/v2/your_username/feeds/your_light_feed
ADAFRUIT_WIFI_SSID_FEED=https://io.adafruit.com/api/v2/your_username/feeds/your_wifi_ssid_feed
ADAFRUIT_WIFI_PASSWORD_FEED=https://io.adafruit.com/api/v2/your_username/feeds/your_wifi_password_feed
ADAFRUIT_SEND_INTERVAL_FEED=https://io.adafruit.com/api/v2/your_username/feeds/your_interval_feed
```

### Frontend Environment (.env.local)
```env
NEXT_PUBLIC_API_URL=http://localhost:3001/api
```

## 🏃‍♂️ Running the Application

### Development Mode

#### Start Backend
```bash
cd backend
npm run start:dev
```
Backend will be available at: `http://localhost:3001`

#### Start Frontend
```bash
cd my-app
npm run dev
```
Frontend will be available at: `http://localhost:3000`

### Test the Integration
```bash
# Test Adafruit connection
curl http://localhost:3001/api/adafruit/health

# Send test sensor data
curl -X POST http://localhost:3001/api/sensors \
  -H "Content-Type: application/json" \
  -d '{
    "deviceId": "esp32-test",
    "temperature": 25.5,
    "humidity": 65,
    "pressure": 1013,
    "timestamp": "2025-01-15T12:00:00Z"
  }'

# Check data in database
curl http://localhost:3001/api/sensors
```

## 📚 API Documentation

### Core Endpoints

#### Devices
- `GET /api/devices` - List all devices
- `GET /api/devices/:id` - Get device by ID
- `POST /api/devices` - Create new device
- `PUT /api/devices/:id` - Update device
- `DELETE /api/devices/:id` - Delete device

#### Sensors
- `GET /api/sensors` - Get sensor data (with filters)
- `GET /api/sensors/latest/:deviceId` - Get latest sensor data
- `GET /api/sensors/statistics/:deviceId` - Get sensor statistics
- `POST /api/sensors` - Create sensor data

#### Control
- `POST /api/control/wifi` - Configure device WiFi
- `POST /api/control/interval` - Set data collection interval
- `POST /api/control/reboot` - Reboot device

#### Adafruit Integration
- `GET /api/adafruit/health` - Check Adafruit connection
- `GET /api/adafruit/config` - Get Adafruit configuration

### WebSocket Events
- `deviceUpdate` - Real-time device updates
- `sensorData` - Real-time sensor data

## 🚀 Deployment

### Backend (Render)
1. Connect GitHub repository to Render
2. Set build settings:
   - Build Command: `npm install`
   - Start Command: `npm run start:prod`
3. Configure environment variables
4. Deploy

### Frontend (Vercel)
1. Connect GitHub repository to Vercel
2. Set root directory to `my-app`
3. Configure environment variables:
   - `NEXT_PUBLIC_API_URL`: Your Render backend URL + `/api`
4. Deploy

### Database (MongoDB Atlas)
1. Create cluster on MongoDB Atlas
2. Create database user
3. Whitelist IP addresses (0.0.0.0/0 for development)
4. Get connection string and update backend environment

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Development Guidelines
- Follow ESLint configuration
- Write meaningful commit messages
- Test your changes thoroughly
- Update documentation as needed

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Adafruit IO](https://io.adafruit.com) for IoT platform
- [NestJS](https://nestjs.com) for backend framework
- [Next.js](https://nextjs.org) for frontend framework
- [MongoDB](https://mongodb.com) for database

## 📞 Support

If you have any questions or issues:

1. Check the [Issues](https://github.com/IOT-DASHBOARD-WEBSITE/your-repo/issues) page
2. Read the [QUICK_START.md](QUICK_START.md) guide
3. Create a new issue with detailed information

---

**🌟 Your IoT Dashboard is ready! Start building amazing IoT applications!**
