# Sunlight Sensor Project

An end-to-end data streaming project including:

- ESP32 driven light sensors, using embedded software written in C with ESP-IDF, 
- Google Cloud Run Functions written in Python and Go, acting as data processing scripts and a REST API,
- BigQuery and Firestore data storage,
- Google Pub/Sub event handling,
- Web app for viewing light intensity levels built with React/Next.js/Typescript, hosted on Firebase
- Monitoring and alerting

## Portfolio Project Goals

In presenting this project, I'm hoping to convince you that:

- As an experienced software engineer, I have a good understanding of the software development cycle and what's involved in developing and deploying production software,
- I can quickly learn and contribute to projects using technologies I was previously unfamiliar with, and
- Working with AI tools effectively and reliably requires incremental change and testing to produce reliable results.  It also takes tenacity to get to the bottom of problems that AI can't solve.

## Project Contents

- [Source Code](#source-code)
- [Web Application](#web-application)
- [Concept](Concept.md)
- [Minimum Viable Product](MinimumViableProduct.md)
- [Use cases](UseCases.md)
- [High-Level Software Architecture](Architecture.md)
- [Statement regarding the use of AI](UseOfAI.md)
- [Nonfunctional Requirements](Nonfunctional.md)
- [CICD](CICD.md)
- [Monitoring](Monitoring.md)
- Sensor Prototype Hardware Builds
  - [Sensor Hardware V1](SensorHardwareV1)
  - [Sensor Hardware V2](SensorHardwareV2)
  - [Enclosure Updates](EnclosureUpdates.md)
- [Test Site Description](Site.md)
- [Important Updates](ImportantUpdates.md)
- [Future Enhancements and Fixes](https://github.com/users/kden/projects/1) Tracked as issues in GitHub

## Source Code

- [GitHub repository for webapp, Cloud Run functions, and Terraform files for Google Cloud Platform
  ](https://github.com/kden/sunlight_sensor_gcp)
- [Github repository for firmware for the ESP32-driven light sensor, using ESP-IDF and C
  ](https://github.com/kden/esp32_sunlight_sensor)

## Web Application

- [Link to the Sunlight Sensor web application.](https://sunlight.codepaw.com/)

At any given time, there may be 1-4 sensors running in the "backyard" sensor set.  I am still in the process of experimenting with data flow timing and power consumption in particular.  The application has test data under the "Test" sensor set, so you can preview it if the sensors are out for maintenance.

- [Web application screen shots](WebappScreenshots.md)
