# Jenkins tool to check DNSSEC signatures in domain zones

This Jenkins pipeline is used to validate DNSSEC signatures in domain zones based on DNS requests. It executes the `python-dnssec-checker` script for each domain specified in the `DOMAINS` parameter.

## Prerequisites

- Jenkins with Build Environment Plugin installed.

## Usage

1. Create a new pipeline in Jenkins and paste the pipeline script.
2. Add the `DOMAINS` parameter in the pipeline configuration with a list of domains to check DNSSEC for, one domain per line.
3. Run the pipeline.

## Pipeline Stages

### Install Python3

This stage updates the package list and installs `Python3` and `Python3-pip` using the apt-get package manager.

### Clone repository

This stage clones the `python-dnssec-checker` Git repository from `https://github.com/AntoOnline/python-dnssec-checker.git`.

### Install requirements

This stage installs the required Python packages listed in the `requirements.txt` file using `pip3`.

### Check DNSSEC for each domain

This stage executes the `python-dnssec-checker` script for each domain specified in the `DOMAINS` parameter. It iterates over the domains, runs the script with the current domain as the argument, and checks the output message. If the output message is not 'OK: there is a valid dnssec self-signed key for the domain,' the stage will fail.

## Pipeline Options

### Build Discarder

This option discards old builds to free up disk space. It uses the `logRotator` class with the `numToKeepStr` parameter set to `1` to keep only the latest build.
