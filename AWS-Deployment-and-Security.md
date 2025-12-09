AWS Deployment Steps and Key Security Configurations

This project was deployed using a fully serverless architecture on AWS.
Below are the detailed deployment steps and the security configurations applied to each component.



**1. DynamoDB Setup**

Deployment Steps:

1.  Created a DynamoDB table named QuotesTable.
2.	Partition key: quote_id (String).
3.	Added 10 initial quote records using the AWS Console.

Security

•	DynamoDB inherits security from IAM roles (no public access).

•	Only the Lambda function is allowed to read from the table through its execution role.

•	No public-read or write access is allowed.



**2. Lambda Function Deployment**

Deployment Steps

1.	Created a new Lambda function (GetRandomQuoteFunction)
2.	Runtime: Python 3.x
3.	Added code to:
	•	Scan DynamoDB table

	•	Select a random quote

	•	Return quote as JSON			
5.	Attached IAM role with permissions to read from DynamoDB.
6.	Tested the function using “Test” in Lambda console.

Security

•	Lambda function runs inside AWS-managed infrastructure (no public exposure).
•	IAM policy only allows:
	•	dynamodb:Scan
	•	dynamodb:GetItem
•	No write or delete permissions were given (principle of least privilege).
•	Environment variables remain empty (no secrets stored in code).



**3. API Gateway Setup**

Deployment Steps

1.	Created an HTTP API (not REST API) for simplicity and low cost.
2.	Created one route:
	•	GET /quote
3.	Integrated this route with the Lambda function.
4.	Deployed the API to a stage named prod.
5.	Tested the API using the invoke URL.

Security

•	The API is public only for read access (GET quotes).
•	No write/update endpoints exist.
•	No API keys required since the app is read-only.
•	HTTPS is enforced automatically by AWS.



**4. S3 Static Website Hosting**

Deployment Steps

1.	Created an S3 bucket named quote-of-the-day-cloud-402
2.	Enabled Static website hosting
3.	Set index document = index.html
4.	Uploaded index.html (frontend)
5.	Set the bucket to “Public website mode” by adding a bucket policy.

Security

•	Block Public Access was turned off ONLY for this bucket because frontend must be public.
•	Public access is limited to:
	s3:GetObject	
•	No write, delete, or upload operations allowed.
•	Bucket does NOT store sensitive data (HTML only).



**5. Connecting Frontend to API Gateway**

Deployment Steps

1.	Updated JavaScript in index.html:
	
   fetch("https://4wcmhpqcik.execute-api.eu-north-1.amazonaws.com/prod/quote")
	
2.	Uploaded updated HTML to S3.
3.	Tested on browser — new quotes load dynamically.

  Security
  
•	Browser can only GET quotes — cannot modify anything.
•	No credentials stored in the frontend.
•	CORS enabled only for the website domain.


    
**6. Overall Security Measures**

- AWS IAM Least Privilege

	Only Lambda can access DynamoDB.

- S3 Bucket Restricted

	Only GetObject is public, nothing else.

- No Hardcoded Secrets

	Project does not require secret keys or tokens.

- Serverless Architecture

	Reduces attack surface (no servers, no SSH, no open ports).



**Summary**

The deployment follows AWS security best practices:

•	Public only where necessary (Static website + GET API)
•	Private everywhere else (DynamoDB, Lambda)
•	IAM least-privilege policies
•	No write operations
•	HTTPS endpoints only
	
This ensures the system is secure, low-cost, and fully serverless.
