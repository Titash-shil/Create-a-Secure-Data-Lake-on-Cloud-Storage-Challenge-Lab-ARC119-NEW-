# Create a Secure Data Lake on Cloud Storage: Challenge Lab || [ARC119](https://www.cloudskillsboost.google/focuses/63857?parent=catalog) ||

# # Like, comment, share & Don't forget to subscribe [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN) 👍😄🤝


* **Note Before Starting The Execution Check and Match Your `Task 1 & Task 4` From Below and Your Instrution page..**
---

### For form 1: Follow below tasks as per your lab instruction & run the commands as per that :
---

* **Task 1. Create a Cloud Storage bucket.**
* **Task 2. Create a lake in Dataplex and add a zone to your lake**
* **Task 3. Environment Creation for Dataplex Lake.**
* **Task 4. Create a tag template (Storage bucket).**

### Run the following Commands in CloudShell

```
export ZONE=
```
```
export REGION="${ZONE%-*}"
export KEY_1=domain_type
export VALUE_1=source_data

gsutil mb -p $DEVSHELL_PROJECT_ID -l $REGION -b on gs://$DEVSHELL_PROJECT_ID-bucket/

gcloud alpha dataplex lakes create customer-lake \
--display-name="Customer-Lake" \
 --location=$REGION \
 --labels="key_1=$KEY_1,value_1=$VALUE_1"

gcloud dataplex zones create public-zone \
    --lake=customer-lake \
    --location=$REGION \
    --type=RAW \
    --resource-location-type=SINGLE_REGION \
    --display-name="Public-Zone"

gcloud dataplex environments create dataplex-lake-env \
           --project=$DEVSHELL_PROJECT_ID --location=$REGION --lake=customer-lake \
           --os-image-version=1.0 --compute-node-count 3  --compute-max-node-count 3

gcloud data-catalog tag-templates create customer_data_tag_template \
    --location=$REGION \
    --display-name="Customer Data Tag Template" \
    --field=id=data_owner,display-name="Data Owner",type=string,required=TRUE \
    --field=id=pii_data,display-name="PII Data",type='enum(Yes|No)',required=TRUE
```

* **Now check your progress, You will get score for all tasks...**
---

### For form 2: Follow below tasks as per your lab instruction & run the commands as per that :
---

* **Task 1. Create a lake in Dataplex and add a zone to your .**
* **Task 2. Environment Creation for Dataplex .**
* **Task 3. Attach an existing Cloud Storage bucket to the zone.**
* **Task 4. Create a tag template (Storage bucket).**

### Run the following Commands in CloudShell

```
export ZONE=
```
```
export REGION="${ZONE%-*}"
export KEY_1=domain_type
export VALUE_1=source_data

gcloud alpha dataplex lakes create customer-lake \
--display-name="Customer-Lake" \
 --location=$REGION \
 --labels="key_1=$KEY_1,value_1=$VALUE_1"

gcloud dataplex zones create public-zone \
    --lake=customer-lake \
    --location=$REGION \
    --type=RAW \
    --resource-location-type=SINGLE_REGION \
    --display-name="Public-Zone"

gcloud dataplex environments create dataplex-lake-env \
           --project=$DEVSHELL_PROJECT_ID --location=$REGION --lake=customer-lake \
           --os-image-version=1.0 --compute-node-count 3  --compute-max-node-count 3

gcloud dataplex assets create customer-raw-data --location=$REGION \
            --lake=customer-lake --zone=public-zone \
            --resource-type=STORAGE_BUCKET \
            --resource-name=projects/$DEVSHELL_PROJECT_ID/buckets/$DEVSHELL_PROJECT_ID-customer-bucket \
            --discovery-enabled \
            --display-name="Customer Raw Data"

gcloud dataplex assets create customer-reference-data --location=$REGION \
            --lake=customer-lake --zone=public-zone \
            --resource-type=BIGQUERY_DATASET \
            --resource-name=projects/$DEVSHELL_PROJECT_ID/datasets/customer_reference_data \
            --display-name="Customer Reference Data"

gcloud data-catalog tag-templates create customer_data_tag_template \
    --location=$REGION \
    --display-name="Customer Data Tag Template" \
    --field=id=data_owner,display-name="Data Owner",type=string,required=TRUE \
    --field=id=pii_data,display-name="PII Data",type='enum(Yes|No)',required=TRUE
```

* **Now check your progress, You will get score for all tasks..**
---

### For form 3: Follow below tasks as per your lab instruction & run the commands as per that :
---

* **Task 1. Create a BigQuery dataset.**
* **Task 2. Add a zone to your lake.**
* **Task 3. Attach an existing BigQuery Dataset to the Lake.**
* **Task 4. Create a tag template (BigQuery Dataset).**

### Run the following Commands in CloudShell

```
export ZONE=
```
```
export REGION="${ZONE%-*}"
bq mk --location=US Raw_data

bq load --source_format=AVRO Raw_data.public-data gs://spls/gsp1145/users.avro

gcloud dataplex zones create temperature-raw-data \
    --lake=public-lake \
    --location=$REGION \
    --type=RAW \
    --resource-location-type=SINGLE_REGION \
    --display-name="temperature-raw-data"

gcloud dataplex assets create customer-details-dataset \
    --location=$REGION \
    --lake=public-lake \
    --zone=temperature-raw-data \
    --resource-type=BIGQUERY_DATASET \
    --resource-name=projects/$DEVSHELL_PROJECT_ID/datasets/customer_reference_data \
    --display-name="Customer Details Dataset" \
    --discovery-enabled

gcloud data-catalog tag-templates create protected_data_template \
    --location=$REGION \
    --display-name="Protected Data Template" \
    --field=id=protected_data_flag,display-name="Protected Data Flag",type='enum(Yes|No)',required=TRUE

echo "${CYAN}${BOLD}Click here: "${RESET}""${BLUE}${BOLD}"https://console.cloud.google.com/dataplex/search?project=$DEVSHELL_PROJECT_ID&q=us-states&qSystems=BIGQUERY""${RESET}"
```

## # For Task 4. Follow below tasks as per your lab instruction & run the commands as per that :
---


* **Task 1. Create a lake in Dataplex and add a zone to your lake.**
* **Task 2. Attach an existing Cloud Storage bucket to the zone.**
* **Task 3. Attach an existing BigQuery Dataset to the Lake.**
* **Task 4. Create Entities.**

### Run the following Commands in CloudShell

```
export ZONE=
```
```
#!/bin/bash
# Define color variables

BLACK=`tput setaf 0`
RED=`tput setaf 1`
GREEN=`tput setaf 2`
YELLOW=`tput setaf 3`
BLUE=`tput setaf 4`
MAGENTA=`tput setaf 5`
CYAN=`tput setaf 6`
WHITE=`tput setaf 7`

BG_BLACK=`tput setab 0`
BG_RED=`tput setab 1`
BG_GREEN=`tput setab 2`
BG_YELLOW=`tput setab 3`
BG_BLUE=`tput setab 4`
BG_MAGENTA=`tput setab 5`
BG_CYAN=`tput setab 6`
BG_WHITE=`tput setab 7`

BOLD=`tput bold`
RESET=`tput sgr0`
#----------------------------------------------------start--------------------------------------------------#

echo "${BG_MAGENTA}${BOLD}Starting Execution${RESET}"

export REGION="${ZONE%-*}"

gcloud alpha dataplex lakes create customer-lake \
--display-name="Customer-Lake" \
 --location=$REGION \
 --labels="key_1=$KEY_1,value_1=$VALUE_1"

gcloud dataplex zones create public-zone \
    --lake=customer-lake \
    --location=$REGION \
    --type=RAW \
    --resource-location-type=SINGLE_REGION \
    --display-name="Public-Zone"

gcloud dataplex assets create customer-raw-data --location=$REGION \
            --lake=customer-lake --zone=public-zone \
            --resource-type=STORAGE_BUCKET \
            --resource-name=projects/$DEVSHELL_PROJECT_ID/buckets/$DEVSHELL_PROJECT_ID-customer-bucket \
            --discovery-enabled \
            --display-name="Customer Raw Data"

gcloud dataplex assets create customer-reference-data --location=$REGION \
            --lake=customer-lake --zone=public-zone \
            --resource-type=BIGQUERY_DATASET \
            --resource-name=projects/$DEVSHELL_PROJECT_ID/datasets/customer_reference_data \
            --display-name="Customer Reference Data"

echo "${CYAN}${BOLD}Click here: "${RESET}""${BLUE}${BOLD}"https://console.cloud.google.com/dataplex/lakes/customer-lake/zones/public-zone/create-entity;location=$REGION?project=$DEVSHELL_PROJECT_ID""${RESET}"

#-----------------------------------------------------end----------------------------------------------------------#
```


# Congratulations ..!!🎉  You completed the lab shortly..😃💯

# *Well done..!* 👏

# Thank you for visiting.... :) 🗯️

# [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN)
