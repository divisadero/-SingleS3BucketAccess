# -SingleS3BucketAccess
Access to a single S3 Bucket with key permissions

1.-Create a new user in IAM AWS console:
   Open AWS with your account.
   Go to IAM service
   Select Users
   Add User > Name > Access type: Programmatic access
   Attach existing policies directly 
   Create Policy:
     Choose a service: S3
     Choose Access level: ex. List+Read+Write
     Click Resources -> Click Add ARN to restrict access
     Write Bucket name
     Click Review Policy
     Write the name of the new policy and description
  Attach policy you just create (clicking Refresh).
  And click little square left of policy and click 'Next: Review'
  Now click Create User button
  If every step is ok, now you are in the key screen. You are given an access key ID and a 
secret access key.

2.-Get your new user ARN:
   Go to IAM service
   Select Users
   Select your user and in the summary you have something like that:
    arn:aws:iam::00000000000:user/your_user_name
   Have it available

3.-Go to the bucket in S3 console:
   Click in Permissions tab > Click On Bucket Policy
   Add this code with arn:aws:iam line change and with bucket_name changed with your own name:
   ```
   {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowMyUser",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::00000000000:user/your_user_name"
            },
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucket_name/*",
                "arn:aws:s3:::bucket_name"
            ]
        }
    ]
}
```

4.-Add the key to the server access credentials:
   (You need install AWS Client if you don't have)
   In ubuntu 16 shell: nano ~/.aws/credentials
   Add this lines at the botton with the credentials you have gotten:
     [AccountAccessBucket]
     aws_access_key_id = AAAAAAAAAAAAAAAAA
     aws_secret_access_key = 1111111111111111111111111
     region = us-east-1
  
5.-Check if it works with this in the command line:
   Changing bucket_name by yours:
   # Listing bucket elements:
   aws s3 ls s3://bucket_name --profile AccountAccessBucket 
   
  
     
