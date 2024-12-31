# Serverless-Text-to-Speech-Application-with-Amazon-Polly

Project Overview:
This project focuses on building a serverless application using AWSservices. The application enables users to send information about a blog post (which is converted into an MP3 file) and retrieve the postdetails, including a link to the audio file stored in an Amazon S3 bucket. The backend is managed entirely through AWS services, eliminating the need for provisioning or managing servers.

Task 1: 
  Create a new DynamoDB table with:

    Table name: posts
    Partition key: id (String)
    Table settings: Default settings

Task 2: Creation of an Amazon S3 bucket to store all audio files created by the application.

Task 3: Creation of an SNS topic
