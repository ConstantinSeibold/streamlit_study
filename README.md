# Rayvolve Study Authentication Application

This project is a Streamlit-based web application designed for authenticated annotation of medical studies. The application allows users to log in, view DICOM images, and annotate them with specific conditions, ensuring that results are securely saved and traceable.

## Features

- **User Authentication**: Secured login using credentials defined in a YAML file (`Users.yml`).
- **DICOM Image Handling**: Load, display, and manipulate DICOM images with windowing features.
- **Annotation Workflow**: Guided annotation process with options for multiple conditions and certainty levels.
- **CSV Export**: Annotations are saved locally and can be downloaded as a CSV file.

## Prerequisites

- Python 3.8 or later
- Docker (optional, for containerized deployment)
- The following Python packages:
  - `streamlit`
  - `pydicom`
  - `numpy`
  - `pandas`
  - `Pillow`
  - `yaml`
  - `streamlit_authenticator`

docker run -d -p 8501:8501 \
  -v /Users/constantinseibold/workspace/rayvolve/Users.yml:/app/Users.yml \
  -v /Users/constantinseibold/workspace/rayvolve/select.yml:/app/select.yml \
  -v /Users/constantinseibold/Downloads/OneDrive_1_10-8-2024/:/app/study_samples \
  -v /Users/constantinseibold/workspace/rayvolve/descriptor.yml:/app/descriptor.yml \
  -v /Users/constantinseibold/workspace/rayvolve/results/:/app/results/ \
  -e USERS_YML=/app/Users.yml \
  -e SELECTS_YML=/app/select.yml \
  -e DESCRIPTOR_YML=/app/descriptor.yml \
  -e FILE_TYPE=dcm \
  -e FILE_PATH=/app/study_samples \
  -e STUDY_MODE=rayvolve \
  -e RUN_MODE=debug \
  cseibold/rayvolve:0.5

## Setup Instructions

### 1. Running Locally via Python

Install the necessary dependencies:

```bash
pip install -r requirements.txt
```

Set the required environment variables:

    FILE_PATH: Path to the folder containing study images.
    STUDY_MODE: Choose between rayvolve and original.
    USERS_YML: Path to the Users.yml file defining user credentials.
    SELECTS_YML: Path to the select.yml file defining annotation options.

Run the application:

```bash
streamlit run rayvolve_study_auth.py
```

### 2. Running via Docker

Build the Docker image (optional):

```bash
docker build -t rayvolve .
```

Run the Docker container via Dockerhub:

```bash
docker run -d -p 8501:8501 \
  -v /path/to/your/Users.yml:/app/Users.yml \
  -v /path/to/your/select.yml:/app/select.yml \
  -v /Users/constantinseibold/Downloads/OneDrive_1_10-8-2024/:/app/study_samples \
  -v /path/to/your/descriptor.yml:/app/descriptor.yml \
  -e USERS_YML=/app/Users.yml \
  -e SELECTS_YML=/app/select.yml \
  -e DESCRIPTOR_YML=/app/descriptor.yml \
  -e FILE_TYPE=dcm \
  -e FILE_PATH=/app/study_samples \
  -e STUDY_MODE=rayvolve \
  -e RUN_MODE=debug \
  cseibold/rayvolve:0.4
```

Example Command:

```bash
docker run -d -p 8501:8501 \
  -v /Users/constantinseibold/workspace/rayvolve/Users.yml:/app/Users.yml \
  -v /Users/constantinseibold/workspace/rayvolve/select.yml:/app/select.yml \
  -v /Users/constantinseibold/Downloads/OneDrive_1_10-8-2024/:/app/study_samples \
  -v /Users/constantinseibold/workspace/rayvolve/descriptor.yml:/app/descriptor.yml \
  -v /Users/constantinseibold/workspace/rayvolve/users.csv:/app/users.csv \
  -e USERS_YML=/app/Users.yml \
  -e SELECTS_YML=/app/select.yml \
  -e DESCRIPTOR_YML=/app/descriptor.yml \
  -e FILE_TYPE=dcm \
  -e FILE_PATH=/app/study_samples \
  -e STUDY_MODE=rayvolve \
  -e RUN_MODE=debug \
  -e ASSIGNMENT_CSV=/Users/constantinseibold/workspace/rayvolve/users.csv \
  cseibold/rayvolve:amd64
```

### Configuration Files

#### Users.yml

Defines user credentials, session management, and case assignments (`startid`,`endid`). Passwords can be in cleartext or hashed. The container will hash cleartext passwords upon startup.

Example Configuration:
```yml
cookie:
  expiry_days: 0
  key: random_signature_key
  name: random_cookie_name
credentials:
  usernames:
    user2:
      email: max.mayer@mail.de
      failed_login_attempts: 0
      logged_in: false
      name: user2
      password: $2b$12$EZIv96oD83iH8Pak9KcF5eOVEXFClFG6nnWZBwh2Sq72MJxqGI9nu
      startid: 0
      endid: 2
```

#### select.yml

```yml
Case_Type:
  - Option 1
  - Option 2
  - Option 3
```

#### descriptor.yml

```yml
study_prefix: "RAY"
login:
  study_name: "Rayvolve - Nutzerstudie"
  username: "Nutzername"
  password: "Password"
  login: "Login"
  logout: "Logout"
  error: "Username/password is incorrect"
  warning: "Please enter your username and password"
task: 
  task_name: "Aufgabenstellung" 
  task_caption: |
    ### Bitte klicken Sie alles an was zutrifft.
    ### Daraufhin erscheint ein Slider. Geben Sie bitte durch diesen Slider an, wie sicher Sie sich dabei sind.
    ### Nachdem Sie Ihre Bewertung angegeben haben, klicken Sie bitte auf 'Submit'.
case: "Case"
selection:
  previous: "Previous"
  next: "Next"
  missing: "Missing:"
  max_missing: 10
certainty: 
  min_certainty: 1
  max_certainty: 5 
  certainty_caption: "Sicherheit: 1 (sehr unsicher) - 5 (sehr sicher) "
comments: "Kommentarfeld"
submit: "Abschicken"
download: "Download Annotation CSV"
```

## Docker Installation Guide

If Docker is not installed on your system, follow the [official Docker installation guide](https://docs.docker.com/engine/install/) to get started.
