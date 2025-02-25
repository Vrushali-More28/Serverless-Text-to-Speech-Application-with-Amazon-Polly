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

Task 6:  Test the functions

      Manually trigger the New Post Lambda function
      It stores data in DynamoDB and send a message to the SNS topic
      SNS triggers the Convert To Audio function, which uses Polly to create an audio file and store it in the S3 bucket    

Task 7: Create a get post Lambda function
      The final Get Post Lambda function provides a method for retrieving information about posts from the database.
  
      Function name: PostReader_GetPost
      Runtime: Python 3.12
      Expand Change default execution role
      Execution role: Choose Use an existing role
      Existing role: Choose Lab-Lambda-Role

Task 8: Expose the Lambda function as a RESTful web service
    
      ◦ Manually triggered the New Post Lambda function, which stored data in DynamoDB and sent a message to Amazon SNS.
      ◦ Verified that SNS successfully triggered the Convert To Audio Lambda function, which used Amazon Polly to generate an audio file and store it in Amazon S3.
      ◦ Checked DynamoDB entries to ensure successful execution and that the post was updated with the S3 audio file URL.
      ◦ Used AWS Lambda monitoring and CloudWatch logs to validate function execution and debug any issues.
      ◦ Downloaded and tested the MP3 file from S3 to confirm correct audio conversion.

Task 9: Create a Serverless User Interface

      ◦ Developed the PostReader_GetPost Lambda function to retrieve post details from DynamoDB.
      ◦ Configured the function using Python 3.12 and assigned the appropriate IAM execution role.
      ◦ Implemented functionality to fetch post details using postId, with an option to retrieve all posts.
      ◦ Set up environment variables for database configuration.
      ◦ Successfully tested the function to verify post retrieval, including S3 URLs for audio files.
