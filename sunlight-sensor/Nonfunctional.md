# Nonfunctional Requirements

## Portfolio technological requirements

There are some requirements that I selected because they are technologies that I am looking for more opportunities to work with.

For example, I wrote some Cloud Functions in Python and at least one in Go. In real life it would probably be better to use the same language for all of them to simplify maintenance, but I wanted to show that I could write a working service in Go.

- Go/Golang
- Embedded programming C/C++
- Python
- React
- Terraform

## Budget constraints

My goal is to pay no more than $10/month on cloud services for this project. I am working within the Google Cloud Platform "always free" tier, which includes some use of:

- Bigquery
- Cloud Functions
- Cloud Run
- Firebase
- Firestore
- Google Pub/Sub

Unfortunately, to meet my budget goals, I could not work with Apache Beam or Spark, which I'm very interested in learning.  CloudSQL, which would give me access to a Postgres instance, is also not free.  Google Cloud Platform allows you to have 1 free Kubernetes cluster, but only 1 free virtual machine to host nodes, making it not very useful for showing features like autoscaling.

One of the challenging realities of a career as a cloud engineer is that there is almost no way to get experience of scale unless you get it on the job. Also, it's challenging to get a job working on a project of scale unless you already have that experience.  As you can see, some of the choices I made were dictated by budget rather than what might be the best if this project were scaled up.
