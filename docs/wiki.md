# Wiki Notes and Documentation

There are two components to this wiki:

- mkdocs
- s3

Both are packages that are installable via python pip. Personally, I like
to install things like this in a `venv`, which creates an isolated python 
environment. This allows you to keep the set of dependencies separate and
isolated from each other, and keeps your local system nice and clean.

At some point, I will add some basic venv documentation. Today is not 
that day.

## MKdocs

Mkdocs is a very simple, markdown-based static website generator. 

Setting up mkdocs is pretty simple, if you're using Linux or OS X. If 
you're using Windows, sorry, I can't help you. 

## s3

Come on. You know what s3 is.

### Set up an s3 bucket with publicly viewable permissions

By default, s3 buckets are private, so to make your static website 
viewable to the world, you have to create a bucket policy that grants
everyone `s3:GetObject` permissions. To do so, in the s3 console of your
AWS account, select the bucket that you will be using, click on `Properties`,
select `Permissions`, select `Edit bucket policy`, and drop the following 
bucket policy into the text field:

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadForGetBucketObjects",
                "Effect": "Allow",
                "Principal": "*",
                "Action": [
                    "s3:GetObject"
                ],
                "Resource": [
                    "arn:aws:s3:::example-bucket/*"
                ]
            }
        ]
    }
    
Don't forget to change "example-bucket" to your bucket name.

### Set up an s3 bucket as a static website

To enable your bucket as a static website, as above, select your bucket 
in the s3 console, select `Properties`, and then select `Static Website Hosting`. 
Check `Enable website hosting`, and select the document that 
will serve as your index document (if you don't know what that means, 
start googling - I'm not covering that here). By default, mkdocs creates 
an index.html file which will serve as the index document, so simply type
`index.html` in the text field for Index Document. Click save, and you 
will be provided with an endpoint, which is the URL you will hit to see 
your wiki. You can also set up a custom domain name, if you have one, but 
that goes beyond the scope of this wiki (it's pretty easy, though, and 
in the AWS documentation).