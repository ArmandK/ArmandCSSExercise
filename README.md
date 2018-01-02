<H3>How to run and test the Cloudformation template</H3>
1. Download the file originalcss.template in your PC
<br>
2. Log in you AWS account that has an admin privileges
<br>
3. Search for CloudFormation service
<br>
4. Click on 'Create Stack' and upload the template from 1. and click 'Next'
<br>
5. Enter a Stack name and the a 'BucketNameField'
<br>
6. Skip 'Options' and click 'Next'
<br>
7. Quickly 'Review' and check the box for 'I acknowledge that AWS CloudFormation might create IAM resources' . Click 'Create'
<br>
8. After the Stack has Status 'CREATE_COMPLETE', go the S3 service and see the Bucket that is created and to the IAM service to see the roles that start with the stack name. You will see two roles.                      NOTE: I previously created a role assumed by a Java Lambda function that will trigger S3 events anytime something happens to the Bucket, but it will be difficult to test for the sake of this exercise (like create another role for the Lambda service, etc..). <B>Simply, both roles are assumed by EC2.</B>
<br>
9. Create a EC2 instance (t2.micro) that you can easily 'ssh' to. <B> I assume that you already have all the Security Settings for that (VPC, subnet, security group with appropriate in/outbound rules, etc..)</B> I recommend to lunch an <b> Amazon Linux AMI.</b> It comes with <b> AWS CLI </b>, since we are going to use this tool to test our S3 policies. While creating the instance, use the 'Auto-assign Public IP' and select one of the roles created.
<br>
10. Assuming that you've selected the <b>'ManageFullyS3Bucket'</b> IAM role for the EC2 and you allowed a <b>Public access to you Bucket from the S3 console</b>, log in to your instance using any SSH client tool, run these commands
<ul>
<li> To <b>VIEW</b> (LIST) object in your bucketEither aws s3 ls s3://yourchosenbucketname OR
 aws s3api list-objects --bucket yourchosenbucketname
</li>

<li> To<b> WRITE</b>, (Upload, PUT) an Object to your buckerEither aws s3 cp /path/to/localfile s3:// yourchosenbucketname OR
 aws s3api put-object --bucket yourchosenbucketname --key /path/to/localfile
</li>

<li> To <b>READ</b>, (Download, GET) an Object to from buckerEither aws s3 cp s3:// yourchosenbucketname  . --recursive  (download into the current directory) OR
 aws s3api get-object --bucket yourchosenbucketname  --key myfile.txt  /path/to/downloadDir/newfile.txt  (This assumes that you've viewed myfile.txt by running $> aws s3 ls s3://yourchosenbucketame OR aws s3api list-objects --bucket yourchosenbucketname)
</li>

<li> To <b>DELETE</b> an Object to from buckerEither aws s3 rm s3:// yourchosenbucketname  . --recursive OR
 aws s3api delete-object --bucket yourchosenbucketname  --key myfile.txt (This assumes that you've viewed myfile.txt by running $> aws s3 ls s3://yourchosenbucketame OR aws s3api list-objects --bucket yourchosenbucketname)
</li>
</ul>
11. Assuming that you've switched to the role <b>'ReadWriteOnS3Bucket'</B> IAM role for the EC2, using Attach/Replace IAM Role from the Instance Settings, perform the same commands as of 10.






