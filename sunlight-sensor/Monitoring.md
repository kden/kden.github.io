# Monitoring

The Sunlight Sensor system sends me an email and SMS message every time one of the sensors has a significant event like capturing the time from an NTP server, which it does at startup.  It also sends me a message whenever it does NOT get a ping from a sensor for more than 15 minutes.  This was implemented by leveraging Google Cloud Platform's alert and monitoring features.

You can see most of the setup in the Terraform monitoring file and the Cloud Run function that generates the log messages for monitoring.