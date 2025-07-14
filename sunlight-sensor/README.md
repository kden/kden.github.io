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

# Statement on the use of AI
I've made liberal use of AI throughout the project.  However, I didn't type in a prompt and let it spit out my project.  It was an 
iterative process which was greatly sped up by using ChatGPT 4o and Google Gemini Pro.  

One reason that I wanted to do a project that interacted with hardware sensors was
that I wanted the code to interact with a hard barrier that AI could not simulate.  I figured it would be easy to ask an LLM to spit out an entire program that looked
like a plausible solution, but if it doesn't interact with anything, you don't know if it works or not.

Also, electronic components and libraries change quickly, and I thought I would probably have to do some troubleshooting and
development that the LLM could not help with.

## LLM Limitations
Gemini and ChatGPT have accelerated my development immensely, but there were some problems they struggled with.
Both struggled with writing React unit tests of any complexity.  They also got confused by converting timezones between 
JavaScript timestamps and Firebase's own timestamp format.  Often Terraform instructions were on the right track, but six months to a year behind, 
and I would have to track down the most recent version of their resource definitions to fix an error. 

Some general problems I ran into:
- Forgetting a source code file I've already shared and generating an entirely new one, deleting all the updates I've been making
- Flipping back and forth between the same two solution fixes that don't work
- Hitting the memory limit when a project reaches a certain size, so that it can't keep track of all the files that a change affects
- Adding "improvements" to your code that you didn't ask for that interfere with the sequential plan for updates you're making

