# Important Updates

This page is a timeline of arbitrarily selected milestones that I felt had some importance.  The first month got us from 0 to a working end-to-end system with a simple event messaging structure and data transformations. 

### August 5, 2025
- Add hourly Open Meteo data, as tables in the web app and as radiation values in the Sensor Levels line graph
- Cloud Run functions all migrated to deploy as GitHub Actions instead of Terraform definitions

## The first month

### July 22, 2025
- Started security updates to use GCP workload identity instead of hard-coded credentials
- Email/SMS monitors set up to alert on sensor outages and important events

### July 17, 2025
- Refactored [sensor firmware](https://github.com/kden/esp32_sunlight_sensor) to use tasks, remove LED display

### July 13, 2025
- Add sunrise, sunset, daily weather data from Open Meteo to web app

### July 12, 2025
- React web app is working with first features: sensor levels and sensor heatmaps charts

### July 3, 2025
- First GitHub actions to deploy React webapp, webapp skeleton

### July 2, 2025
- Completed first version of [ingestion API](https://github.com/kden/sunlight_sensor_gcp/tree/main/functions/rest_sensor_api_to_pubsub) which saved sensor data to BigQuery

### July 1, 2025
- Completed first version of [sensor firmware](https://github.com/kden/esp32_sunlight_sensor) which sends data from a sensor to an API endpoint

### June 23, 2025
- "Hello world" commit to  [sensor firmware](https://github.com/kden/esp32_sunlight_sensor) project