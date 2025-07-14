# Sunlight Sensor Project

## Concept
[Permaculture](https://en.wikipedia.org/wiki/Permaculture) is a sustainable approach to land use, emphasizing a self-sustaining, regenerative ecosystem.  Farmers and 
gardeners are experimenting with permaculture techniques to use land in a way more compatible with natural ecosystems.

One important concept in permaculture planning is [microclimates](https://en.wikipedia.org/wiki/Microclimate).  A microclimate is a small area that has a different climate than the surrounding area. 
If you have a small yard, but want to incorporate features of different
ecosystems, you can find or create microclimates that support different plants and animals.  

To plan what plants to put in a microclimate, you need to know things like the amount of sunlight, humidity, and temperature. 
Sensors for these conditions have become rather inexpensive, and it's possible that a homeowner or 
hobby farmer could use them to map their various microclimates.

## Minimal Viable Product
For me, whose idea of gardening is to throw gobs of native plant wildflower seeds around the yard and see what sticks, 
implementing permaculture is daunting.  I experimented for a few years in replacing our grass with clover as that seemed environmentally responsible.
However, clover doesn't grow well in shady ares.  I also bought a number of bare-root raspberry plants.  The ones that I planted beneath trees have been dormant for several years, but the ones I planted in the sun blew up in just a year.  

I decided that sunlight was the most important data for improving the backyard.  **The minimal viable product, then, will be a small set of 
sensors in the backyard that tracks the amount of sunlight in different areas.**

In the future, we can add sensors for temperature and humidity, and larger numbers of sensors, but this will be a good proof of concept.

## Use cases
- As a gardener or small farmer 
  - **I want to be able to look at an app on my tablet or desktop and see what parts of my land get the most sun.**
    - I would like to see the total sun exposure of various parts of the yard over the course of a day, a week, or a month
    - I would like to see the sun exposure of the yard at a specific time.
  - I would like to see the readings on a sensor over the course of a day.
  - I would like a way to see which sensors are down, for example, if they are out of batteries.


## Portfolio technological requirements
There are some requirements that I selected because they are technologies that I am looking for more opportunities to work with.
For example, I wrote some Cloud Functions in Python and at least one in Go.  In real life it would probably be better to use the same 
language for all of them to simplify maintenance, but I wanted to show that I could write a working service in Go.
- Go/Golang
- Embedded programming C/C++
- Python
- React
- Terraform

## Budget constraints
My goal is to pay no more than $10/month on cloud services for this project.  I am working within the Google Cloud Platform "always free" tier, which includes some use of:
- Bigquery
- Cloud Functions
- Cloud Run
- Firebase
- Firestore
- Google Pub/Sub

Unfortunately, to meet my budget goals, I could not work with Apache Beam or Spark ETL pipelines, which I'm very interested in learning.
CloudSQL, which would give me access to a Postgres instance, is also not free.

One of the challenging realities of a career as a cloud engineer is that there is almost no way to get experience of scale unless you get it on 
the job.  Also, it's challenging to get a job working on a project of scale unless you already have that experience.
