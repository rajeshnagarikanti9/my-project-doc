Serverless Media Processing Pipeline on AWS

1. Introduction
In this project, I developed a serverless media processing pipeline using AWS services. The system is designed to automatically process images uploaded by users without requiring any manual intervention or server management.
Whenever an image is uploaded, the system resizes it and adds a watermark before storing it in a separate location. This ensures efficient handling of large-scale media uploads in a scalable and cost-effective manner.


2. Objective
The main objective of this project is to build an automated pipeline that:
•	Accepts image uploads securely 
•	Processes images using AWS Lambda 
•	Stores processed images separately 
•	Optimizes storage costs using lifecycle rules 
•	Ensures data durability with cross-region replication 
•	Supports scalable and event-driven architecture 


3. Architecture Overview
The system follows an event-driven workflow:
•	The user uploads an image to an S3 bucket 
•	The upload triggers a Lambda function 
•	The Lambda function resizes the image and adds a watermark 
•	The processed image is stored in another S3 bucket 
•	The image can be delivered globally using CloudFront 




4. AWS Services Used
The following AWS services were used in this project:
•	Amazon S3 (for storage) 
•	AWS Lambda (for processing images) 
•	Lambda Layers (Pillow library) 
•	S3 Lifecycle Rules (for cost optimization) 
•	S3 Cross-Region Replication (for durability) 
•	CloudFront (for global delivery – optional) 
•	SQS DLQ (for error handling – optional) 


5. Implementation Steps

Step 1: Creating the Raw Upload Bucket
An S3 bucket was created to store incoming images.
•	Versioning enabled 
•	SSE-S3 encryption enabled 
•	CORS configured for uploads 

Step 2: Creating the Processed Images Bucket
A second S3 bucket was created to store processed images.
•	SSE-KMS encryption enabled 

Step 3: Configuring Cross-Region Replication
Cross-region replication was configured to replicate data to another region for backup and disaster recovery.



Step 4: Setting Lifecycle Rules
Lifecycle rules were added to reduce storage costs:
•	30 days → Standard-IA 
•	90 days → Glacier 
•	365 days → Delete 


Step 5: Creating the Lambda Function
A Lambda function named image-processor was created.
•	Runtime: Python 3.11 
•	Memory: 512 MB 
•	Timeout: 30 seconds 



Step 6: Configuring IAM Permissions
An IAM execution role was attached to allow the Lambda function to access S3 buckets.
<img width="940" height="499" alt="image" src="https://github.com/user-attachments/assets/f3d8b3f2-3ffd-4ba1-90e6-18bf8522b090" />


Step 7: Creating Lambda Layer (Pillow)
A Lambda layer was created to include the Pillow library for image processing.
Steps performed:
•	Installed Pillow using Docker 
•	Packaged it into a ZIP file 
•	Uploaded as Lambda layer 
•	Attached to the function 
<img width="940" height="412" alt="image" src="https://github.com/user-attachments/assets/9145dedf-1255-409d-8d75-b4224ee3fe7e" />


Step 8: Adding S3 Trigger
The Lambda function was configured to trigger automatically when a new object is uploaded to the raw bucket.


Step 9: Writing Lambda Code
The Lambda function performs the following tasks:
•	Reads image from S3 
•	Resizes it to 800x600 
•	Adds a watermark 
•	Uploads processed image 

Step 10: Testing the Pipeline
An image was uploaded to the raw bucket to test the pipeline.
<img width="940" height="316" alt="image" src="https://github.com/user-attachments/assets/09bd9364-4dd1-490c-a629-b694239ffb39" />
<img width="940" height="330" alt="image" src="https://github.com/user-attachments/assets/a2a03ecd-eb6a-403c-bae2-ada6a91f8a22" />


6. Results
The system worked successfully:
•	Images were processed automatically 
•	Lambda was triggered correctly 
•	Processed images were stored in the destination bucket 

10. Conclusion
This project demonstrates how AWS serverless services can be used to build a scalable and automated media processing system.
The solution is efficient, cost-effective, and suitable for real-world applications that handle large volumes of user-generated content.


