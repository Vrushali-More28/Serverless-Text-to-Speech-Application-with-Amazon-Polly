# Serverless-Text-to-Speech-Application-with-Amazon-Polly

Project Overview:
This project focuses on building a serverless application using AWSservices. The application enables users to send information about a blog post (which is converted into an MP3 file) and retrieve the postdetails, including a link to the audio file stored in an Amazon S3 bucket. The backend is managed entirely through AWS services, eliminating the need for provisioning or managing servers.

Task 1: 
  Create a new DynamoDB table with:

    Table name: posts
    Partition key: id (String)
    Table settings: Default settings
    
  After completion the application setup, it stores the following information in the DynamoDB table:
    
    id: The ID of the post.
    status: UPDATED or PROCESSING, depending on whether an MP3 file has already been created.
    text: The post's text, for which an audio file is being created.
    voice: The Amazon Polly voice that was used to create audio file.
    url: A link to an S3 bucket where an audio file is being stored.

Task 2: Create an Amazon S3 bucket to store all audio files created by the application.

Task 3: Creation of an SNS topic
    It allows the application to use asynchronous calls so that the user who sends a new post to the application immediately receives the ID of the new DynamoDB item, so it knows what to ask       for later without having to wait for the conversion to finish. With small posts, the process of converting to audio files can take milliseconds, but with bigger posts (100,000 words or         more), converting the text can take longer.

Task 4: Create a new post Lambda function
    The first Lambda function you create is the entry point for the application. It receives information about new posts that should be converted into audio files.
    The Lambda function does the following:
      
      Retrieves two input parameters:
      Voice: One of dozens of voices that are supported by Amazon Polly
      Text: The text of the post that we want to convert into an audio file
      Creates a new record in the DynamoDB table with information about the new post
      Publishes information about the new post to SNS (the ID of the DynamoDB item/post ID is published there as a message)
      Returns the ID of the DynamoDB item to the user
      
Task 5: Create a convert to audio Lambda function that is stored in the DynamoDB table into an audio file.
    The Lambda function does the following:
      
      Retrieves the ID of the DynamoDB item (post ID) which should be converted into an audio file from the input message (SNS event)
      Retrieves the item from DynamoDB
      Converts the text into an audio stream
      Places the audio (MP3) file into an S3 bucket
      Updates the DynamoDB table with a reference to the S3 bucket and the new status
    
