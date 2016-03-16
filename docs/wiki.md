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

To install mkdocs, execute the following command:

`pip install mkdocs`

If you aren't using a venv, you'll probably have to sudo that. After installing, 
create a directory for your mkdocs projects:

`mkdir projects`

Cd into that directory, and create your first mkdocs project!

`mkdocs new project`

This creates a directory that will contain the files for your mkdocs project. 
The initial directory contains a yaml document, a directory, and a markdown
file inside of the directory:

    example/
    ├── docs
    │   └── index.md
    └── mkdocs.yml

The mkdocs.yml file is essentially the project index (not to be confused 
with the index.html page), in which the site name, project structure and theme are
defined. The files contained in the docs directory are the files that 
the static html files will be created from. If you were to write "Hello 
World" in the index.md, and then execute the following command:

`mkdocs build`

you would find a new directory, called site, inside your project directory.
This would contain html, css, javascript, and whatever else is necessary
for your website.

To preview your site, there is a built in webserver, that auto-updates as
you work on your site. To launch this, run the `mkdocs serve` command:

    $ mkdocs serve
    Running at: http://127.0.0.1:8000/

That's a very basic overview of how to get started with mkdocs - for 
full documentation visit [mkdocs.org](http://mkdocs.org).

**Commands**

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs help` - Print this help message.

**Project layout**

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

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

### Workflow

The basic workflow is as follows:

1. Create and/or edit a page
2. Run `mkdocs serve` to preview what's being worked on
3. Once I'm happy, kill the server and run `mkdocs build --clean` to 
build the site - the --clean flag gets rid of any files that are no longer needed
4. Push the contents of the site directory to s3, using aws cli (the 
command I use is `aws s3 --profile rog sync . s3://mybucketname`)
5. Commit and push the changes to git (put the site dir in the .gitignore)
