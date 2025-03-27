# Setting Up Datasources in Grafana

Before importing the earthquake dashboard, you need to set up the required datasources in Grafana:

## 1. Prometheus Datasource

1. Log in to Grafana (<http://localhost:3000>) with admin/admin
2. Go to **Configuration** > **Data Sources**
3. Click **Add data source**
4. Select **Prometheus**
5. Configure the datasource:
   - Name: `Prometheus` (important: this exact name is used in the dashboard)
   - URL: `http://prometheus:9090` (if using Docker Compose) or your Prometheus server URL
   - Access: Server (default)
6. Click **Save & Test** to verify the connection

## 2. WorldMap Plugin and Datasource

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

## 3. Import the Dashboard

Now that you have set up the required datasources:

1. Go to **Dashboards** > **Import**
2. Upload the `earthquake-dashboard.json` file or paste its contents
3. Click **Import**

## Troubleshooting

If you still see "$DS_PROMETHEUS not found" errors:

1. Edit the dashboard JSON directly in a text editor
2. Replace all instances of `${DS_PROMETHEUS}` with `prometheus`
3. Replace all instances of `${DS_WORLDMAP}` with `worldmap`
4. Save the file and import again

Alternatively, when importing the dashboard in Grafana, you can manually map the variables to your datasources in the import UI.
