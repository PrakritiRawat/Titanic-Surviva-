🐳 Streamlit & PostgreSQL Dashboard in Docker 🐘📊

  

📌 Overview

Deploy a Streamlit application that connects to a PostgreSQL database, fully containerized with Docker. This end-to-end solution covers:

Securely running PostgreSQL in a Docker container

Configuring a custom Docker network for inter-container communication

Building and deploying a Streamlit dashboard in Docker

Displaying and filtering passenger data from PostgreSQL

🚀 Features

🔗 Inter-container Networking: Custom Docker network to link Streamlit and PostgreSQL containers

🐘 PostgreSQL Backend: Containerized, persistent database for passenger data

📈 Streamlit UI: Real-time data visualization and filtering of passenger records

📂 Easy Setup: Single-network setup, minimal commands, developer-friendly

🛠️ Prerequisites

Docker (>=20.10) & Docker Compose (optional) 🐳

Python 3.9+ (for local testing) 🐍

Basic familiarity with command-line interfaces

📁 Repository Structure

Streamlit-Postgres-Docker/
├── Dockerfile              # Streamlit app container definition
├── main.py                 # Streamlit application code
└── students.csv            # Sample data for local testing (optional)

⚙️ Setup Steps

1️⃣ Create a Docker Network

docker network create my_postgres_network

Enables Streamlit and PostgreSQL containers to communicate by name.

2️⃣ Launch PostgreSQL Container

docker run -d \
  --name postgres-db \
  --network my_postgres_network \
  -e POSTGRES_USER=appuser \
  -e POSTGRES_PASSWORD=s3cr3t \
  -e POSTGRES_DB=passengerdb \
  -v pgdata:/var/lib/postgresql/data \
  postgres:13

Volume pgdata ensures data persists across restarts.

3️⃣ Initialize Database Schema

docker exec -it postgres-db psql -U appuser -d passengerdb <<EOF
CREATE TABLE passengers (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  location VARCHAR(100)
);
INSERT INTO passengers (name, location) VALUES
  ('Alice','New York'),
  ('Bob','Los Angeles'),
  ('Charlie','Chicago');
EOF

4️⃣ Build the Streamlit App Image

docker build -t streamlit-dashboard .

Your Dockerfile installs dependencies (streamlit, psycopg2-binary) and copies main.py.

5️⃣ Run the Streamlit Container

docker run -d \
  --name streamlit-app \
  --network my_postgres_network \
  -p 8501:8501 \
  streamlit-dashboard

🌐 Access the Dashboard

Open your browser at:👉 http://localhost:8501/

You’ll see a live list of passengers fetched directly from your PostgreSQL database.

📝 main.py (Key Highlights)

import streamlit as st
import psycopg2
import pandas as pd

# Database connection settings
db_config = {
    'host': 'postgres-db',
    'port': 5432,
    'dbname': 'passengerdb',
    'user': 'appuser',
    'password': 's3cr3t'
}

# Connect and fetch data
def load_data():
    conn = psycopg2.connect(**db_config)
    df = pd.read_sql('SELECT * FROM passengers', conn)
    conn.close()
    return df

st.title("🛫 Passenger Dashboard")

df = load_data()
st.write(df)

min_loc = st.text_input("Filter by location:")
if min_loc:
    st.write(df[df['location'].str.contains(min_loc, case=False)])


