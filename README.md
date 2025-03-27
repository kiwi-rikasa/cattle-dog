# Earthquake Monitoring Dashboard

This repository contains configuration files for setting up a complete earthquake monitoring system using Grafana, Prometheus, and AlertManager.

## Components

- **Grafana**: Visualization platform for earthquake data
- **Prometheus**: Time-series database for storing earthquake metrics
- **AlertManager**: Alert handling system for earthquake notifications
- **Earthquake Exporter**: (Placeholder) Service that collects earthquake data from sources like USGS

## Files

- `earthquake-dashboard.json`: Grafana dashboard configuration
- `prometheus-earthquake.yml`: Prometheus configuration file
- `earthquake_rules.yml`: Prometheus alerting rules
- `alertmanager.yml`: AlertManager configuration
- `docker-compose.yml`: Docker Compose configuration to run the entire stack
- `datasource-setup.md`: Guide for setting up required Grafana datasources

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

5. Set up datasources in Grafana (see `datasource-setup.md` for detailed instructions):
   - Add a Prometheus datasource named `prometheus` pointing to your Prometheus server
   - Install the WorldMap panel plugin and configure a datasource named `worldmap`

6. Import the dashboard:
   - In Grafana, go to Dashboards > Import
   - Upload the `earthquake-dashboard.json` file or copy-paste its contents
   - If prompted to select datasources, map them correctly to your configured datasources

## Troubleshooting

If you see an error like `datasource ${DS_PROMETHEUS} not found`, follow these steps:

1. Make sure you've set up the Prometheus datasource with the name `prometheus`
2. Make sure you've installed the WorldMap panel plugin and set up a datasource named `worldmap`
3. Alternatively, edit the `earthquake-dashboard.json` file and replace:
   - All instances of `${DS_PROMETHEUS}` with `prometheus`
   - All instances of `${DS_WORLDMAP}` with `worldmap`

See `datasource-setup.md` for more detailed instructions on setting up datasources.

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
