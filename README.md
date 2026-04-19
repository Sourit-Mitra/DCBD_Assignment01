# DCBD_Assignment01
DCBD Assignment: Analyzing Publication Metadeba via RPC
Student ID: MDS202537
Course: Distributed Computing and Big Data (DCBD)

Overview
This project implements a MapReduce-style pipeline to analyze publication metadata fetched from a remote RPC server. The goal is to identify the top 10 most frequent first words across 1000 publication titles retrieved via HTTP API calls.

Problem Statement
Given a remote server hosting 1000 publication files (pub_0.txt to pub_999.txt), each containing a title, the task is to:

Authenticate with the server using a student ID to obtain a secret key
Fetch the title of each publication via the /lookup endpoint
Extract the first word from each title
Use a MapReduce approach with multiprocessing to count word frequencies
Identify the top 10 most frequent first words and verify via the /verify endpoint


Approach
MapReduce Pipeline
PhaseDescriptionMapEach worker processes a chunk of files, fetches titles, extracts first words, and builds a local CounterReduceAll local counters are merged into a single global Counter
Key Design Decisions

Multiprocessing: Used Python's multiprocessing.Pool with up to 4 workers to parallelize HTTP requests
Rate Limiting: Implemented exponential backoff on HTTP 429 responses and a small per-request delay (0.05s) to stay within server limits
Retry Logic: Each fetch retries up to 3 times on failure before returning None
Shared Secret Key: One master secret key is obtained at startup and reused across all workers to avoid redundant logins


Results
MetricValueTotal files processed1000Unique first words found24Verification score10 / 10 ✅
Top 10 First Words:
RankWordCount1Advanced942Analytical813Comprehensive754Automated655Distributed596Dynamic597Fundamental578Heuristic569Experimental5110Global50

Project Structure
DCBD_Assignment01/
├── Dockerfile          # Docker container definition
├── README.md           # This file
├── REQUIREMENT.txt     # Python dependencies
└── run.py              # Main application script

How to Run
Prerequisites

Python 3.11-slim
requests library

Install dependencies:
bashpip install -r REQUIREMENT.txt
Run Locally
bashpython run.py
Run via Docker
Build the image:
bashdocker build -t myapp .
Run the container:
bashdocker run myapp
Export as .tar for submission(submitted separately, not incuded in GitHub repository):
bashdocker save myapp -o Firstname_MDS202537_Assignment01.tar

API Endpoints Used
EndpointMethodPurpose/loginPOSTAuthenticate with student ID, receive secret key/lookupPOSTFetch title for a given publication filename/verifyPOSTSubmit top 10 words for scoring
Base URL: http://72.60.221.150:8080

Verification Output
{'correct': True, 'message': 'Congratulations! You found all top 10 first words.', 'score': 10, 'total': 10}
