## download 3 MIMIC III input files to ./code/data
cd {your path}/clinical-notes-diagnosis-dl-nlp
1. ./data/NOTEEVENTS-2.csv (contains clinical notes / text)
2. ./data/DIAGNOSES_ICD.csv (contains admission to ICD9 code diagnosis)
3. ./data/D_ICD_DIAGNOSES.csv (contains the ICD9 code description)

# clone git source
    cd /workspace
    git clone https://github.com/tiratano/clinical-notes-diagnosis-dl-nlp.git

# Build Docker image from Dockerfile
    docker build -t mimic3_icd9_coding.v1 .

# Run the Docker image in a new container
    docker run -v /workspace/clinical-notes-diagnosis-dl-nlp:/workspace/clinical-notes-diagnosis-dl-nlp -it mimic3_icd9_coding.v1 bin/bash

