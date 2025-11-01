# IoT Monitoring System

A Docker-based IoT monitoring system that combines MQTT messaging, InfluxDB time-series database, and Grafana
visualization for real-time data collection and monitoring.

## Architecture

The system consists of the following components:

- MQTT Broker (Eclipse Mosquitto)
- InfluxDB time-series database
- MQTT Publisher (Java-based)
- MQTT to InfluxDB Scrapper
- Grafana dashboard

## Prerequisites

- Docker and Docker Compose
- Java Development Kit (JDK) for building custom images
- Git

## Setup

1. Clone the repository
2. Create the following secret files:
    - `.env.influxdb2-admin-username`
    - `.env.influxdb2-admin-password`
    - `.env.influxdb2-admin-token`
3. Configure environment variables in `.env` file
4. Build the custom Docker images:
   ```bash
   docker build -t mqtt-pub-java:latest ./mqtt-publisher
   docker build -t mqtt-influx-scrapper:latest ./mqtt-scrapper
   ```
5. Start the services:
   ```bash
   docker-compose up -d
   ```

## Environment Variables

| Variable         | Description                     | Default Value          |
|------------------|---------------------------------|------------------------|
| INFLUXDB_ORG     | InfluxDB organization name      | pathvariable           |
| INFLUXDB_BUCKET  | InfluxDB bucket name            | smartgarden            |
| GRAFANA_USER     | Grafana admin username          | admin                  |
| GRAFANA_PASSWORD | Grafana admin password          | adminpassword          |
| MQTT_TOPIC       | MQTT topic to subscribe/publish | nursery                |
| INFLUXDB_TOKEN   | InfluxDB authentication token   | MyInitialAdminToken0== |
| GOOGLE_API_KEY   | Google AI Studio API Key        | none, need account     |

Note: Google Ai Studio has a free tier for demo purposes

## Services

### MQTT Broker (Mosquitto)

- Port: 1884 (external), 1883 (internal)
- Anonymous access enabled
- Persistent storage for messages and logs

### InfluxDB

- Port: 8086
- Secured with username/password
- Data and config persistence

### Grafana

- Port: 3001 (external), 3000 (internal)
- Pre-configured with InfluxDB data source
- Customizable dashboards

## Usage

1. Access Grafana at `http://localhost:3001`
2. Login with credentials from environment variables
3. Access the existing dashboard that is connected to the InfluxDB datasource

## Data Flow

1. IoT devices publish data to MQTT broker
2. MQTT Scrapper subscribes to topics and forwards data to InfluxDB
3. Grafana queries InfluxDB and displays data in dashboards
