# Steps to running our docker image

## set environments variables
    export DLH_PRJ_BASE=/workspace  # your_path

## clone git source    
    cd $DLH_PRJ_BASE  # ex) /workspace
    git clone https://github.com/tiratano/clinical-notes-diagnosis-dl-nlp.git

## download 3 MIMIC III input files to ./code/data
cd $DLH_PRJ_BASE/clinical-notes-diagnosis-dl-nlp
1. ./code/data/NOTEEVENTS-2.csv (contains clinical notes / text)
2. ./code/data/DIAGNOSES_ICD.csv (contains admission to ICD9 code diagnosis)
3. ./code/data/D_ICD_DIAGNOSES.csv (contains the ICD9 code description)

## download the docker image and load it (OneDrive)
    docker load -i mimic3_prep_docker

## Run the docker image with a docker volume
    docker run -v $DLH_PRJ_BASE/clinical-notes-diagnosis-dl-nlp:$DLH_PRJ_BASE/clinical-notes-diagnosis-dl-nlp -it mimic3_icd9_coding.v1 bin/bash

## Run the preprocess.py
    cd $DLH_PRJ_BASE/clinical-notes-diagnosis-dl-nlp/code
    python3 ./preprocess.py
    
if you run with real data and are connected to a remote server, it would be better to run with a nohup command

    cd $DLH_PRJ_BASE/clinical-notes-diagnosis-dl-nlp/code
    nohup /root/anaconda3/bin/python3.8 ./preprocess.py > logs.txt 2>&1 &

----------------------
# Docker build steps 

## Build Docker image from Dockerfile
    docker build -t mimic3_icd9_coding.v1 .

## Run the Docker image with a attached volume
    docker run -v $DLH_PRJ_BASE/clinical-notes-diagnosis-dl-nlp:$DLH_PRJ_BASE/clinical-notes-diagnosis-dl-nlp -it mimic3_icd9_coding.v1 bin/bash

## Modify source in your host machine 
cd {your path}/clinical-notes-diagnosis-dl-nlp

    # if you just want to see what's going on, use NOTEEVENTS-2-mini.csv not NOTEEVENTS-2.csv
    # NOTEEVENTS-2-mini.csv has only 10 lines so you can easily check the errors or results

## Run the preprocess.py in docker container
cd {your path}/clinical-notes-diagnosis-dl-nlp

    python3 $DLH_PRJ_BASE/clinical-notes-diagnosis-dl-nlp/code/preprocess.py

## Now you can get the below results.
1. ./data/DATA_HADM (contains top 50 icd9code, top 50 icd9cat, and clinical text for each admission)
2. ./data/DATA_HADM_CLEANED (contains top 50 icd9code, top 50 icd9cat, and cleaned clinical text w/out stopwords for each admission)
3. ./data/ICD9CODES.p (pickle file of all ICD9 codes)
4. ./data/ICD9CODES_TOP10.p (pickle file of top 10 ICD9 codes)
5. ./data/ICD9CODES_TOP50.p (pickle file of top 50 ICD9 codes)
6. ./data/ICD9CAT_TOP10.p (pickle file of top 10 ICD9 categories)
7. ./data/ICD9CAT_TOP50.p (pickle file of top 50 ICD9 categories)

## Etc.
1. If you want to debug in a local machine, you can run the docker commands and run the code on the local PC.
2. Maybe there is a way to debug a code in the docker container, but I don't know the method yet.

## docker image export / import
    # image export    
    docker save -o mimic3_prep_docker mimic3_icd9_coding.v1

    # image import
    docker load -i mimic3_prep_docker

