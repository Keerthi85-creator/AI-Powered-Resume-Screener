### AI Resume Analyzer

[[Live Demo](https://ai-powered-resume-screener-4mrnkvkguvthemkpsjhtjh.streamlit.app/)]

AI Resume Analyzer is a production-ready Streamlit application that analyzes resumes using explainable, rule-based logic and semantic similarity techniques. It provides users with structured resume insights such as skill extraction, experience estimation, resume scoring, job-role matching, and learning recommendations, while offering administrators advanced analytics, resume deduplication, similarity search, and clustering powered by persisted embeddings in MongoDB.

## Key Features

### User Features
- Upload resumes in PDF format with in-app preview
- Automatic resume text extraction using PDF parsing
- Dictionary-based skill extraction with normalization and aliases
- Rule-based experience level detection
- Explainable resume quality scoring with detailed breakdown
- Job role selection and semantic job match scoring
- Skill gap analysis against role-specific requirements
- Curated course and interview preparation recommendations
- Duplicate resume detection using semantic hashing
- Resume analysis persisted for analytics
- User feedback submission with rating and comments
- Feedback data persisted in MongoDB for admin analysis
- About page with contact message submission
- Contact messages stored in MongoDB for review


### Admin Features
- Secure admin access with session-based authentication
- Event-based analytics stored in MongoDB
- Experience-level and role-wise performance insights
- Global and role-specific missing skills analysis
- Resume similarity search using stored embeddings
- KMeans-based resume clustering for internal analysis
- Cluster-level insights for grouped resumes
- CSV export of analytics data
- hardcoded admin password - admin123


## System Architecture

The application follows a strict modular architecture with a clear separation between the frontend, backend logic, and data persistence layers.

- **Frontend (Streamlit)**  
  Handles routing, navigation, and user interaction. Application entry and navigation are centralized in `app/main.py`, while user-facing and admin-facing logic are isolated into dedicated view modules.

- **Backend (Python)**  
  All core logic is implemented in the `backend/` directory and organized by responsibility, including resume parsing, analysis, NLP processing, recommendations, and database access.

- **Database (MongoDB)**  
  MongoDB is used as the primary persistence layer. Resumes are stored as deduplicated entities, while analytics are stored as event-based records linked to resumes. This design enables scalable analytics without duplicating resume data.

- **Intelligence Layer**  
  Lightweight NLP techniques are used for semantic similarity, resume deduplication, and clustering. Sentence embeddings are stored in the database to support similarity search and admin-only clustering workflows.

## Technology Stack

- **Frontend**: Streamlit  
- **Backend**: Python  
- **Database**: MongoDB  
- **NLP**: SentenceTransformers (all-MiniLM-L6-v2)  
- **PDF Parsing**: pdfminer  
- **Similarity & Clustering**: Cosine Similarity, KMeans  
- **Visualization**: Streamlit charts  
- **Environment Management**: Python environment variables  

<h2 id="project-structure">Project Structure</h2>

<details>
<summary>Click to expand project structure</summary>

```text
ai_resume_analyzer/
│
├── app/                         # Streamlit UI layer
│   ├── main.py                  # Main Streamlit entry point
│   ├── pages/                   # Multiple Streamlit pages
│   │   ├── user.py
│   │   ├── admin.py
│   │   ├── feedback.py
│   │   └── about.py
│   ├── assets/                  # Logos, images
│   └── components/              # UI helper components
│
├── backend/                     # All backend logic
│   ├── parser/
│   │   ├── resume_parser.py
│   │   └── pdf_reader.py
│   │
│   ├── nlp/
│   │   ├── embeddings.py
│   │   ├── similarity.py
│   │   ├── skill_extractor.py
│   │   └── job_matcher.py
│   │
│   ├── recommender/
│   │   ├── skill_recommender.py
│   │   ├── course_recommender.py
│   │   └── tips.py
│   │
│   ├── analysis/
│   │   ├── resume_score.py
│   │   ├── experience_level.py
│   │   └── clustering.py
│   │
│   ├── database/
│   │   ├── db.py
│   │   ├── user_data.py
│   │   └── feedback_data.py
│   │
│   └── utils/
│       ├── helpers.py
│       └── constants.py
│
├── data/
│   ├── courses.json
│   ├── skills.json
│   └── samples/
│
├── Uploaded_Resumes/
├── README.md
├── requirements.txt
└── .gitignore
```
</details>


## Data Model Overview

<details>
<summary>Model overview</summary>
The system uses a clear separation between core entities and event-based analytics to ensure data integrity, scalability, and meaningful insights.

### Resume (Entity)
- Each resume is stored **once** as a unique entity.
- Duplicate resumes are detected using a semantic hash generated from normalized resume text.
- Resume records store:
  - Parsed resume data
  - Semantic embedding
  - Cluster identifier (admin analytics)
- This prevents redundant storage and enables reuse across analytics and intelligence layers.

### Analytics (Events)
- Analytics are stored as **event-based records**.
- Each resume analysis generates a new analytics event linked via `resume_id`.
- Analytics records include:
  - Experience level
  - Resume score
  - Target role
  - Job match score
  - Skill presence and gaps
  - Timestamp
- This design supports historical tracking and trend analysis without duplicating resume data.

This entity–event separation allows the system to scale analytics independently while keeping resume intelligence centralized and consistent.
</details>

### Installation Steps

1. Clone the repository

   ```bash
   git clone https://github.com/your-username/AI_Resume_Analyzer.git
   cd AI_Resume_Analyzer
2. Create and activate a virtual environment:
   ```bash
   python -m venv venv
   source venv/Scripts/activate   # Windows

3. Install dependencies:
    ```bash
    pip install -r requirements.txt

4. Run the application:
    ```bash
    steamlit run app/main.py


## MongoDB Setup



This project uses MongoDB as the primary persistence layer for resume data, analytics, feedback, and admin insights.



### 1. Create a MongoDB Atlas Account



1. Visit https://www.mongodb.com/atlas

2. Create a free MongoDB Atlas account.

3. Create a free M0 cluster.



### 2. Create a Database User



1. Navigate to **Database Access**.

2. Click **Add New Database User**.

3. Create a username and password.

4. Assign **Read and Write to Any Database** or **Atlas Admin** privileges.



### 3. Configure Network Access



1. Navigate to **Network Access**.

2. Click **Add IP Address**.

3. For development environments such as GitHub Codespaces or Streamlit Cloud, add:



```text

0.0.0.0/0

```



4. Save the configuration and wait for Atlas to apply the changes.



### 4. Obtain the Connection String



1. Open your cluster.

2. Click **Connect** → **Drivers**.

3. Select **Python**.

4. Copy the generated connection string.



Example:



```text

mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/resume_analyzer?retryWrites=true&w=majority

```



### 5. Create Environment Variables



Create a `.env` file in the project root:



```env

MONGODB_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/resume_analyzer?retryWrites=true&w=majority

```



### 6. Verify Connection



Start the application:



```bash

streamlit run app/main.py

```



If the application launches successfully, MongoDB has been configured correctly.



### Important



* Do NOT commit your `.env` file.

* Do NOT share MongoDB usernames or passwords publicly.

* The `.env` file is already excluded through `.gitignore`.

* Each developer should use their own MongoDB Atlas account and connection string.


## Current Status

- Core user-facing resume analysis is complete and feature-locked.
- Admin analytics, resume similarity, and clustering are fully implemented and stable.
- Resume data is deduplicated and persisted in MongoDB.
- Analytics are stored as event-based records for historical insights.
- The application is deployment-ready and compatible with Streamlit Cloud.


## Future Scope

- MongoDB Atlas integration for cloud deployment.
- Job Description (JD) analysis and resume–JD comparison.
- Cluster-level insights to identify common strengths and gaps across resumes.
- Advanced admin visualizations for long-term trend analysis.
- Optional resume feedback export for users.

<h2 id="preview" align="center">Preview</h2>
<hr>

#### Home Page
<p align="center">
  <img src="docs/screenshots/home1.png" width="900">
  <img src="docs/screenshots/home2.png" width="900">
</p>

### User Page
<p align="center">
  <img src="docs/screenshots/navbar_and_user_inf.png" width="900">
</p>

## Resume Summary, Score, and Breakdown with Target Role Selection

<p align="center">
  <img src="docs/screenshots/resume_target_job_role.png" width="900">
</p>

## Skill Gap Analysis & Job Match Score

<p align="center">
  <img src="docs/screenshots/skill_gap_resume_score.png" width="900">
</p>


### Skill Development & Career Support

<!-- Overview -->
<p align="center">
  <img src="docs/screenshots/recomendations.png" width="900">
</p>

<!-- Full width -->
<p align="center">
  <img src="docs/screenshots/recom1.png" width="900">
</p>

<!-- Side by side -->
<p align="center">
  <img src="docs/screenshots/recom2.png" width="45%">
  <img src="docs/screenshots/recom3.png" width="45%">
</p>


### Admin Page & Insights

## Admin Login (Password Required)
<p align="center">
  <img src="docs/screenshots/admin_password_inf.png" width="45%">
  <img src="docs/screenshots/incorrect_paddword.png" width="45%">
</p>

## Admin Dashboard Overview
<p align="center">
  <img src="docs/screenshots/system_overview.png" width="900">
</p>

## User Ratings and Rating Distribution

<p align="center">
  <img src="docs/screenshots/rating_chart.png" width="900">
</p>

## Missing Skills (Overall and Role-wise)

<p align="center">
  <img src="docs/screenshots/admin_insights.png" width="900">
</p>

<p align="center">
  <img src="docs/screenshots/ingights.png" width="900">
</p>

## Resume and Role Distributions
<p align="center">
  <img src="docs/screenshots/distributions1.png" width="900">
  <img src="docs/screenshots/distribution2.png" width="900">
</p>

## Resume Similarity Analysis
<p align="center">
  <img src="docs/screenshots/resume_similarity.png" width="900">
</p>

## Resume Clustering Based on Admin-Selected Number of Clusters
<p align="center">
  <img src="docs/screenshots/clustering1.png" width="900">
  <img src="docs/screenshots/clustering2.png" width="900">
</p>

## Download Analytics Data
<p align="center">
  <img src="docs/screenshots/download_analytics.png" width="900">
</p>

### Feedback Page

<p align="center">
  <img src="docs/screenshots/feedback1.png" width="900">
  <img src="docs/screenshots/feedback2.png" width="900">
</p>


###  About and Contact Page

<p align="center">
  <img src="docs/screenshots/contact.png" width="900">
</p>

## Conclusion

AI Resume Analyzer is a fully functional, modular resume analysis system designed with scalability, explainability, and real-world usability in mind.  
The project demonstrates practical applications of NLP, analytics, and system design using Streamlit and MongoDB.


## Contributions

Contributions, suggestions, and feedback are welcome.  
Feel free to open an issue or submit a pull request.
















