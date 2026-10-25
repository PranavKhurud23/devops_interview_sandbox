**
High-Level GCP Application Flow**

<img width="544" height="355" alt="image" src="https://github.com/user-attachments/assets/3705f958-01e6-45ee-89a2-4704be660441" />


User request reaches the Load Balancer, which forwards traffic to Ingress. Ingress routes the request to the appropriate Kubernetes Service, and the Service forwards it to the application Pod.
The application processes the request. If additional or background processing is required, events are published to Pub/Sub for asynchronous execution. Files or raw data are stored in Cloud Storage, while structured information and metadata are stored in the database. 
Cloud Logging and Cloud Monitoring provide observability throughout the process, and the response is returned back to the user through the same path.


**GCP PubSUB**

<img width="353" height="224" alt="image" src="https://github.com/user-attachments/assets/c8d72004-e16a-471e-9b98-dc7e2e435f7d" />


User Uploads File
        │
        ▼
Application Pod
        │
        ├── Store File in Bucket
        │
        ├── Save Metadata in Database
        │
        └── Publish Event to Pub/Sub
                    │
                    ▼
         Multiple Consumers

         ├─ Virus Scan Service
         ├─ Notification Service
         └─ Audit Service

         
For example, when a user uploads a file, the application stores the file in Cloud Storage and saves metadata in a database. 
If additional processing such as virus scanning, notifications, or auditing is required, an event is published to Pub/Sub. Multiple subscribers can then process the event independently.


# GCP DevOps & SRE IaC Infrastructure


<img width="450" height="269" alt="image" src="https://github.com/user-attachments/assets/80efa61e-7e68-4745-86bf-4d19b519bb5b" />
<img width="280" height="326" alt="image" src="https://github.com/user-attachments/assets/da10b368-7f2c-4bdc-834d-996f939fcafb" />


# Write a Simple VPC Network

resource "google_compute_network" "vpc" {
    name = "test-vpc"
    auto_create_subnetwork = false
}
resource "google_compute_subnetwork" "sbnet" { 
     name = "test-subnet"
     ip_cidr_range = "10.1.0.0/24"
     region = asia-south-1
     google_compute_network.vpc.id
}
# Create Cluster with this VPC

resource "google_container_cluster" "primary" {
     name = "job-cluster"
     region = "asian-south"
     project_id  = sdfdsdsfs or var.porject_id
     remove_default_node_pool =  true
     initial_node_count = 1
     network = google_compute_network.vpc.name
     subnetwork = google_compute_subnetwork.sbnet.name
}


**Terraform Interview Question**
https://github.com/gashok13193/DevOps-Docs/tree/main/Terraform
https://media.licdn.com/dms/document/media/v2/D4D1FAQG--wS2ZfWpYw/feedshare-document-pdf-analyzed/feedshare-document-pdf-analyzed/0/1714179048376?e=1793836800&v=beta&t=qory1AZ0nAVKUICpl-wZSRP48ntkmws3JsSHx-6rfR4
