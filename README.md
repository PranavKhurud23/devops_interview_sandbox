# GCP DevOps & SRE IaC Infrastructure


<img width="450" height="269" alt="image" src="https://github.com/user-attachments/assets/80efa61e-7e68-4745-86bf-4d19b519bb5b" />


+-------------------------------------------------------------+
               |                   GCS REMOTE BACKEND                        |
               |       (Stores terraform.tfstate & locks state)             |
               +------------------------------+------------------------------+
                                              ^
                                              | Configured in providers.tf
                                              v
+---------------------------------------------------------------------------------------------------+
| ROOT CONFIGURATION (Environment: Dev / Prod)                                                       |
|                                                                                                   |
|  [ terraform.tfvars ] --------> Injects actual values (region="asia-south1", nodes=3)             |
|                                         |                                                         |
|  [ variables.tf ] ------------> Declares inputs required for this environment                     |
|                                         |                                                         |
|  [ main.tf ] -----------------> Orchestrates build & wires modules together                      |
|       |                                 |                                                         |
+-------|---------------------------------|---------------------------------------------------------+
        | Calls Module                    | Passes VPC Output to GKE Module
        v                                 v
+---------------------------------------------------------------------------------------------------+
| REUSABLE MODULES DIRECTORY (modules/)                                                             |
|                                                                                                   |
|   +--------------------------+               +--------------------------+                         |
|   |   modules/vpc/           |               |   modules/gke/           |                         |
|   |                          |               |                          |                         |
|   |   • main.tf (Resources)  |               |   • main.tf (Resources)  |                         |
|   |   • variables.tf (Inputs)|               |   • variables.tf (Inputs)|                         |
|   |   • outputs.tf (Export)  |               |   • outputs.tf (Export)  |                         |
|   +------------+-------------+               +------------+-------------+                         |
|                |                                          |                                       |
|                +-------------> Exports vpc_id ------------+                                       |
+---------------------------------------------------------------------------------------------------+
                                          |
                                          | Creates Actual Resources in GCP
                                          v
+---------------------------------------------------------------------------------------------------+
| GOOGLE CLOUD PLATFORM (GCP)                                                                       |
|                                                                                                   |
|   [ VPC Network & Subnets ]  ===============>  [ GKE Regional Cluster (asia-south1) ]          |
+---------------------------------------------------------------------------------------------------+
