#  Quote of the Day – Serverless Web App
A fully serverless motivational quote generator using AWS DynamoDB, Lambda, API Gateway, and S3.

**Overview**
This project is a serverless web application that displays a random motivational quote each time the page loads or the user clicks “New Quote.”


It uses:

•	DynamoDB to store quotes
	
•	Lambda to select a random quote
	
•	API Gateway to expose the Lambda function
	
•	S3 Static Website Hosting to serve the frontend
	
•	IAM roles & policies to secure the application

**Architecture Diagram**

   S3 Website (HTML/CSS/JS) --> API Gateway (GET /quote) --> Lambda Function --> DynamoDB Table


**Features**

•	Fully serverless (no servers to manage)
	
•	Low cost (Lambda + DynamoDB + S3)
	
•	Secure (IAM least-privilege model)
	
•	Random quote retrieval
	
•	Fast API response using HTTP API Gateway

**Project Structure**
      **quote-of-the-day-cloud-project/**
      - lambda/
        - lambda_function.py
	   
**frontend/**
        -  index.html
      - **config/**
  		-  bucket-policy.json
		-  dynamodb-schema.json
   		-  lambda-role-policy.json
    
 -  **screenshots/**
  		-  (AWS screenshots)
		
  	


**Frontend**

The frontend is pink-themed HTML page hosted on Amazon S3:

Live Website:

http://quote-of-the-day-cloud-402.s3-website-us-east-1.amazonaws.com

The page uses JavaScript to fetch a random quote:

fetch("https://4wcmhpqcik.execute-api.eu-north-1.amazonaws.com/prod/quote")




**Backend Logic**

Lambda Function

Located in lambda/lambda_function.py.
	•	Scans DynamoDB table
	•	Selects a random quote
	•	Returns JSON response

Example Response:

{
  "quote_id": "5",
  "text": "It always seems impossible until it is done.",
  "author": "Nelson Mandela"
}



**AWS Deployment Steps**

1. DynamoDB Setup
	•	Created table: QuotesTable
	•	Partition key: quote_id (String)
	•	Inserted 10 sample quotes

2. Lambda Deployment
	•	Function: GetRandomQuote
	•	Runtime: Python 3.x
	•	Environment variable:
TABLE_NAME = QuotesTable
	•	IAM role grants read-only DynamoDB access

3. API Gateway Deployment
	•	Created HTTP API
	•	Route: GET /quote
	•	Lambda integration
	•	Stage: prod
	•	Public endpoint:
https://4wcmhpqcik.execute-api.eu-north-1.amazonaws.com/prod/quote

4. S3 Website Hosting
	•	Bucket: quote-of-the-day-cloud-402
	•	Static website hosting enabled
	•	Public read-only access via bucket policy
	•	Uploaded index.html



**Security Configurations**

✔ IAM Least Privilege

Lambda only has permission to:

dynamodb:Scan

✔ S3 Bucket Public Access

Only GetObject is public:
No write, delete, or upload allowed.

✔ HTTPS Everywhere

API Gateway uses HTTPS by default.

✔ No Secrets Exposed

Frontend contains no credentials.

✔ DynamoDB Fully Private

Only readable through Lambda (never public).



**Screenshots**

Screenshots of each AWS component are included in:

screenshots/

They demonstrate:
	•	DynamoDB table
	•	Lambda code
	•	Lambda test response
	•	API Gateway route
	•	API Gateway stage + invoke URL
	•	S3 bucket hosting
	•	Bucket policy
	•	Final hosted website



**How to Run Locally (Optional)**

You can open frontend/index.html in any browser.
Quotes will still load as long as the API Gateway URL is correct.



This project demonstrates best practices in AWS serverless design:
	•	Minimal cost
	•	Highly secure
	•	Scalable
	•	Clean architecture
	•	Real-time API integration
	•	Polished UI


