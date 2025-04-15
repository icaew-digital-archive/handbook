# AWS CLI

[NOTE: Add a "Overview" section]
[NOTE: Add a "Installation" section]
[NOTE: Add a "Configuration" section]
[NOTE: Add a "Basic Commands" section]
[NOTE: Add a "Security" section]
[NOTE: Add a "Best Practices" section]

# Uploading large files via AWS CLI 

The easiest way to upload large files to the Preservica PUT area is via the AWS CLI tool. It can be dowloaded from [here](https://docs.aws.amazon.com/cli/index.html).

After installation, run:

        aws configure  
            
and enter the Preservica authentication details which can be found on the [Logins page](../../logins.md).


Usage is via standard Linux commands, for instance, list the contents of the directory:
        
        aws s3 ls s3://com.preservica.icaew.put.holding/    

Upload/copy a local file to s3:

        aws s3 cp '/home/digital-archivist/Downloads/20220719-ICAEW-com-media-library.zip' s3://com.preservica.icaew.put.holding/20220719-ICAEW-com-media-library.zip

Upload/copy a local directory to s3:
        
        aws s3 cp --recursive local_directory s3://com.preservica.icaew.put.holding/NewFolder/

List aws folder contents with their etags and size (note this example uses com.preservica.icaew.export):

        aws s3api list-objects --bucket com.preservica.icaew.export