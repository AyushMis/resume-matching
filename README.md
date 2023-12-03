# Design Document

A. Architecture diagram of the overall system



Description
- User Interface (UI): Provides a user-friendly interface for users to input job descriptions and upload resumes. 
- Job Description Parser: Extracts structured information from the provided job description. This information includes job title, location, industry/domain, education degree, and technical skills. 
- Resume Parser: Extracts structured information from the uploaded resumes. This includes job title, location, industry/domain, education degree, and technical skills. 
- Matching Engine: Compares the extracted attributes from the job description and resume. It calculates a similarity score for each attribute, indicating how well the resume matches the job description. 
- Scoring Algorithm: Assigns a weighted score to each attribute based on its importance to the job. The overall match score is calculated as a weighted sum of individual attribute scores. 
- Database: Stores and manages job descriptions, resumes, and historical matching data for future analysis and improvement.

B. Component Design: 
- Job Description Parser: Utilizes natural language processing (NLP) techniques to extract relevant information from the job description. Employs Named Entity Recognition (NER) for identifying entities like job title, location, and organization. Regular expressions are used to capture specific patterns related to education and technical skills. 
- There are chances that Job title identified with the NER won’t be having high accuracy, so we can use publicly available data here at kaggle to train our model to get which role/field are we referring to in Job description.
- Resume Parser: Applies NLP techniques to extract structured data from resumes. Utilizes NER for entities like job title, location, and organization. Regular expressions are employed for extracting education details and technical skills.
- Matching Engine: Compares the extracted attributes from the job description and resume. Employs a combination of exact matching, fuzzy matching, and semantic matching for a robust comparison. Assigns weights to different attributes based on their relevance to the job. 
- Scoring Algorithm: Calculates a similarity score for each attribute based on the matching approach used. Assigns weights to different attributes based on their importance to the job. Aggregates individual attribute scores to calculate the overall match score. 

C. Attribute Extraction and Matching Details: 
- Job Title: Extracted using NER and regular expressions. Matching involves comparing the extracted job titles between the job description and resume. 
- Location: Extracted using NER. Matching involves comparing the extracted locations between the job description and resume. 
- Industry/Domain: Extracted using NER. Matching involves comparing the extracted industry/domain information between the job description and resume. 
- Education Degree: Extracted using a combination of NER and regular expressions. Matching involves comparing the extracted education details between the job description and resume. 
- Technical Skills: Extracted using a combination of NER and regular expressions. Matching involves comparing the extracted technical skills between the job description and resume. 

Conclusion: The resume matching and scoring system outlined above leverages NLP techniques, regular expressions, and a scoring algorithm to provide a comprehensive analysis of how well a given resume matches a specific job description. The architecture ensures flexibility, scalability, and the ability to adapt to various job domains and requirements. The system can be continuously improved by analyzing historical data and refining the matching algorithms based on user feedback. 



# Deployment Doc

Prerequisites
Docker: Ensure that Docker is installed on the machine where you plan to deploy the microservice. 
Docker Compose (Optional): If your system involves multiple containers, consider installing Docker Compose.

Steps: 
1. Containerize the Application: 
Create a Dockerfile in the root directory of your project.
##### Use an official Python runtime as a parent image 
FROM python:3.8-slim 

##### Set the working directory to /app 
WORKDIR /app 

##### Copy the current directory contents into the container at /app 
COPY . /app 

##### Install any needed packages specified in requirements.txt 
RUN pip install --no-cache-dir -r requirements.txt 

##### Make port 80 available to the world outside this container 
EXPOSE 80 

##### Define environment variable 
ENV NAME World 

##### Run app.py when the container launches 
CMD ["python", "app.py"]


Update the requirements.txt file with the necessary dependencies such as
spacy==3.1.3
regex==2022.11.10
torch==1.10.0
transformers==4.11.3
scikit-learn==0.24.2

Build the Docker image:
	docker build -t resume-matching-service .

2. Run the Docker Container:
Run the Docker container:
	docker run -p 4000:80 resume-matching-service

Now, the microservice should be accessible at http://localhost:4000 (adjust the port according to your configuration). 
3. Docker Compose (Optional): 
If your microservice involves multiple containers, you can use Docker Compose for managing the multi-container Docker applications. Create a docker-compose.yml file:

version: '3' 
services: 
   resume-matching-service: 
     build: . 
     ports: 
       - "4000:80"


Run the application using Docker Compose:
Docker-compose up

4. Deployment to a Container Orchestration System (Optional): 
For production deployments, consider using container orchestration systems like Kubernetes or Docker Swarm. These systems help manage the deployment, scaling, and operation of containerized applications. 
Create Kubernetes deployment files (e.g., deployment.yaml, service.yaml) and deploy to a Kubernetes cluster. 
For Docker Swarm, use docker stack deploy. 

5. Continuous Integration/Continuous Deployment (CI/CD): 
Consider integrating CI/CD pipelines to automate the building, testing, and deployment processes. 

Conclusion: This documentation provides a basic guide to containerizing and deploying the resume matching and scoring system as a microservice using Docker. Adjustments may be needed based on your specific requirements and the technologies used in your application. Always follow security best practices when deploying microservices in production environments.
