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

# Rediness vs Liveness 

<img width="389" height="215" alt="image" src="https://github.com/user-attachments/assets/c9830d1e-9437-410e-bed3-157190c219b2" />


# GCP DevOps & SRE IaC Infrastructure


<img width="450" height="269" alt="image" src="https://github.com/user-attachments/assets/80efa61e-7e68-4745-86bf-4d19b519bb5b" />
<img width="280" height="326" alt="image" src="https://github.com/user-attachments/assets/da10b368-7f2c-4bdc-834d-996f939fcafb" />


# Write a Simple VPC Network


<img width="678" height="215" alt="image" src="https://github.com/user-attachments/assets/4456ad76-f47f-451b-8068-b24b6b4c5cd1" />

<img width="658" height="350" alt="image" src="https://github.com/user-attachments/assets/0f3173c8-90d6-40a6-974d-045e3960376b" />

<img width="671" height="353" alt="image" src="https://github.com/user-attachments/assets/48e99e08-23fb-422b-bd36-f156c89f568a" />



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

# For Service Account
resource "google_service_account" "sa" {
   account-id = "sanity-sa"
   display_name = "APP SA for WA" 
}

# For Bucket Creation 
<img width="436" height="315" alt="image" src="https://github.com/user-attachments/assets/3a282c06-da03-4cb0-b723-50e07ca9b326" />




# Add Node In it

resource "google_container_node_pool "node-p" {
    name = "job-node"
    location = "asia-south"
    project = var.project_id
    cluster = google_container_cluster.primary.name
    node_count = 1

   node_config {
   machine-type = "e2-medium"
   disk_size_gb = 30
   oauth_scope = [define auth link]
}

}

# Custome  metric & Dashboard
Business wants a dashboard showing successful transactions.In application logs, whenever a transaction succeeds, we see: "Transaction Success"
<img width="419" height="194" alt="image" src="https://github.com/user-attachments/assets/a9acf6b4-8b62-4bd7-a27d-7213b5a7525f" />

<img width="464" height="293" alt="image" src="https://github.com/user-attachments/assets/bfed4974-97bc-4257-99c9-b9249f6dd5ea" />

<img width="439" height="312" alt="image" src="https://github.com/user-attachments/assets/243d8ae2-d7dd-466c-b678-e08d3322e557" />

<img width="425" height="149" alt="image" src="https://github.com/user-attachments/assets/9d6c23de-adfb-4d8f-a7bb-1cbcdfccb826" />


GAUGE
↓
Current Value (Right Now)

Examples:
• CPU Usage = 65%
• Memory Usage = 4 GB
• Active Pods = 10

Question:
"What is the value currently?"


DELTA
↓
New Changes During a Period

Examples:
• Successful Transactions Today = 500
• Errors Generated This Hour = 25
• API Calls Last 5 Minutes = 1000

Question:
"How many new events happened during this period?"


CUMULATIVE
↓
Total Since Start

Examples:
• Total Transactions Since App Started = 1,000,000
• Total Requests Processed Since Startup = 50,000
• Total Messages Consumed Since Service Started = 200,000

Question:
"What is the overall running total?"







# Prod CI/CD flow

<img width="537" height="290" alt="image" src="https://github.com/user-attachments/assets/9b370a90-d7ea-466f-9ba4-4b554ca86d26" />



# DockerFile

<img width="493" height="386" alt="image" src="https://github.com/user-attachments/assets/15f479bf-5ede-442b-bdde-df0547be8b15" />

<img width="454" height="314" alt="image" src="https://github.com/user-attachments/assets/82103b31-2bca-4603-b270-946e46e7ee75" />

# Helm 

<img width="677" height="350" alt="image" src="https://github.com/user-attachments/assets/26e55ac5-02a9-45f3-97f5-06997c1668ef" />

# Script upload to bucket
<img width="696" height="337" alt="image" src="https://github.com/user-attachments/assets/593923a5-e804-492c-b131-63c9a4eb37de" />


**Terraform Interview Question**
https://github.com/gashok13193/DevOps-Docs/tree/main/Terraform
https://media.licdn.com/dms/document/media/v2/D4D1FAQG--wS2ZfWpYw/feedshare-document-pdf-analyzed/feedshare-document-pdf-analyzed/0/1714179048376?e=1793836800&v=beta&t=qory1AZ0nAVKUICpl-wZSRP48ntkmws3JsSHx-6rfR4

<img width="550" height="254" alt="image" src="https://github.com/user-attachments/assets/ae4519b5-bb04-4410-8e35-2136cf4e715f" />

# Troubleshoot 
Production Application Down Troubleshooting Path
<img width="390" height="316" alt="image" src="https://github.com/user-attachments/assets/d21902bd-0c84-4992-a825-68ed0d0a5507" />

**Perfomrance troubleshoot**

<img width="555" height="325" alt="image" src="https://github.com/user-attachments/assets/91a990a5-c16c-4034-873e-24e14c64cf9f" />

**Missing Data**

<img width="524" height="302" alt="image" src="https://github.com/user-attachments/assets/12727a2c-0700-49e6-87bd-9785df221068" />




