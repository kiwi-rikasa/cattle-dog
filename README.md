# Earthquake Monitoring Dashboard

This repository contains configuration files for setting up a complete earthquake monitoring system using Grafana, Prometheus, and AlertManager.

## Components

- **Grafana**: Visualization platform for earthquake data
- **Prometheus**: Time-series database for storing earthquake metrics
- **AlertManager**: Alert handling system for earthquake notifications
- **Earthquake Exporter**: Service that collects earthquake data from sources like USGS

## Installation

1. Ensure Docker and Docker Compose are installed on your system.
2. Clone this repository:

   ```shell
   git clone <repository-url>
   cd earthquake-monitoring
   ```

3. Start the stack:

   ```shell
   docker-compose up -d
   ```

4. Access Grafana:
   - URL: <http://localhost:3000>
   - Username: admin
   - Password: admin

5. Set up datasources in Grafana (see detailed instructions below)
6. Import the dashboard (see detailed instructions below)

## Setting Up Datasources in Grafana

Before importing the earthquake dashboard, you need to set up the required datasources in Grafana:

### 1. Prometheus Datasource

1. Log in to Grafana (<http://localhost:3000>) with admin/admin
2. Go to **Configuration** > **Data Sources**
3. Click **Add data source**
4. Select **Prometheus**
5. Configure the datasource:
   - Name: `prometheus` (important: this exact name is used in the dashboard)
   - URL: `http://prometheus:9090` (if using Docker Compose) or your Prometheus server URL
   - Access: Server (default)
6. Click **Save & Test** to verify the connection

### 2. WorldMap Plugin and Datasource

The earthquake dashboard uses the WorldMap Panel plugin which needs to be installed:

1. Go to **Configuration** > **Plugins**
2. Search for "WorldMap Panel"
3. Click on the plugin and click **Install**
4. Restart Grafana if needed

After installing the plugin, set up a datasource for it:

1. Go to **Configuration** > **Data Sources**
2. Click **Add data source**
3. Search for and select **Grafana WorldMap Panel**
   - If you can't find it, any JSON or Prometheus datasource can work with the WorldMap panel
4. Name it `worldmap` (important: this exact name is used in the dashboard)
5. Configure according to your data needs
6. Click **Save & Test**

### 3. Import the Dashboard

Now that you have set up the required datasources:

1. Go to **Dashboards** > **Import**
2. Upload the `earthquake-dashboard.json` file or paste its contents
3. Click **Import**

## Troubleshooting

If you see an error like `datasource ${DS_PROMETHEUS} not found`, follow these steps:

1. Make sure you've set up the Prometheus datasource with the name `prometheus`
2. Make sure you've installed the WorldMap panel plugin and set up a datasource named `worldmap`
3. Alternatively, edit the `earthquake-dashboard.json` file and replace:
   - All instances of `${DS_PROMETHEUS}` with `prometheus`
   - All instances of `${DS_WORLDMAP}` with `worldmap`

If you still see "$DS_PROMETHEUS not found" errors:

1. Edit the dashboard JSON directly in a text editor
2. Replace all instances of `${DS_PROMETHEUS}` with `prometheus`
3. Replace all instances of `${DS_WORLDMAP}` with `worldmap`
4. Save the file and import again

Alternatively, when importing the dashboard in Grafana, you can manually map the variables to your datasources in the import UI.

## Alerts

The system comes with pre-configured alerts:

- **High Magnitude Earthquake**: Triggers when an earthquake with magnitude greater than 6.0 is detected
- **Moderate Magnitude Earthquake**: Triggers when an earthquake with magnitude between 5.0 and 6.0 is detected
- **Earthquake Data Collection Failure**: Triggers when the data collector is down for more than 5 minutes
- **Increased Seismic Activity**: Triggers when a region experiences more than 10 earthquakes in 24 hours

## Customization

- Modify `alertmanager.yml` to configure your own notification channels (email, Slack, PagerDuty, etc.)
- Adjust alert thresholds in `earthquake_rules.yml`
- Customize the dashboard in Grafana after importing

## Data Source

This setup uses a placeholder for the earthquake data exporter. In a real-world scenario, you would need to implement or use an existing exporter that fetches earthquake data from sources like:

- [USGS Earthquake API](https://earthquake.usgs.gov/fdsnws/event/1/)
- [EMSC](https://www.emsc-csem.org/service/rss/)
- [GeoNet](https://www.geonet.org.nz/data/types/earthquake)

The exporter should convert this data into Prometheus metrics format.
