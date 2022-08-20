# local-simplesite-httpservers

Demonstrate various simple http-servers for local web development which
includes:

- npm [http-server: a simple static HTTP server](https://www.npmjs.com/package/http-server)

- npm [serve](https://www.npmjs.com/package/serve)

- Python [http-server](https://docs.python.org/3/library/http.server.html)

In addition, we show deployment of the simple website to:

- Amazon S3

- Amazon S3 + CloudFront

- "Render.com"

## Prerequisite

- [Node.js & NPM](https://digitalcompanion.gitbook.io/home/setup/node.js)

- [Python](https://digitalcompanion.gitbook.io/home/setup/dev-environment/python)


## Setup

- Clone this repo

- Note: the `src` folder contains a very simple web site that will be served
  by the simple http-server


## Serving website locally

### npm http-server

- Make sure `node.js` exists
```
npm --version
8.11.0
```
 
- Install the npm `http-server` module
```
npm install http-server
```

- Run the Http server
```
npm run http-server

# alternative
npx http-server ./src -p 8000
```

- Open browser and go to `http://localhost:8000`

- CTRL-C to terminate

### npm serve

- Make sure `node.js` exists
```
npm --version
8.11.0
```
 
- Install the npm `serve` module
```
npm install serve
```

- Run the Http server
```
npm run serve
```

- Open browser and go to `http://localhost:8000`

- CTRL-C to terminate

### python server

- Make sure Python exists
```
python3 --version
Python 3.8.10
```

- Make the script executable
```
chmod +x run-py-server.sh
```

- Run the Http server
```
./run-py-server.sh
```


## Deploy

### Amazon S3

**NOTE**: Amazon S3 website endpoints do not support HTTPS. If you want to use 
HTTPS, use Amazon CloudFront to serve a static website hosted on Amazon S3.

#### Manual deployment from AWS Console

Source: [Tutorial: Configuring a static website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html)

- Sign in to the AWS Management Console and go to the Amazon S3 console at
  [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/).
  The default S3 console page should be the "Buckets" page showing list of
  existing buckets.

- Create a new bucket:
  - enter the Bucket name (for example, `example.com`). For more info:
    - For bucket to work with CloudFront, the name must conform to DNS naming 
      requirements.
    - see bucket [Naming rules](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html)
    - see bucket [Restrictions and limitations](https://docs.aws.amazon.com/AmazonS3/latest/userguide/BucketRestrictions.html)
  - choose the Region to create the bucket
  - accept the default settings and create the bucket, choose Create.

- Enable website hosting from the newly created bucket:
  - choose the name of the bucket to enable static website hosting for
  - choose Properties.
  - from the "Static website hosting" panel, choose Edit.
  - for the "Static website hosting" selections, choose Enable.
  - in Index document, enter the file name of the index document, 
    typically `index.html`. Note: the index document name is case sensitive and 
    must exactly match the file name of the HTML index document that you plan 
    to upload to your S3 bucket.
  - to provide the custom error document for 4XX class errors, enter the custom
    error document file name.
  - choose Save changes.
  - Amazon S3 is enabled to serve the static website from the bucket. At the 
    bottom of the page, under Static website hosting, the website endpoint for 
    the bucket can be found. Use this endpoint to test the website.

- Unblock public access settings:
  - By default, Amazon S3 blocks public access to your account and buckets; even
    though the bucket has been enabled for website hosting
  - To "custom unblock", choose Permissions tab
  - From the "Block public access (bucket settings)" panel, choose Edit
  - Clear the "Block all public access" checkkbox, and choose Save changes.
  - Confirm

- Add a bucket policy to grant public read access to your bucket. When you grant
  public read access, anyone on the internet can access your bucket:
  - Under Bucket Policy panel, choose Edit.
  - To grant public read access for the website, paste the following Bucket 
    policy into the editor:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}
```    

- Upload an index and error documents, and other dependencies:
  - from the Object tab, click Upload files: `index.html`, `error.html`,
    `favicon-32px.png` and `styles.css`.

- Test by entering the website endpoint to the browser. The endpoint can be 
  found from the "Properties tab > Static website hosting panel > Bucket website
  endpoint".


### Amazon S3 & CloudFront

![architecture-aws-s3-cloudfront](/images/architecture-aws-s3-cloudfront.png)

Main Source: [Getting started with a simple CloudFront distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/GettingStarted.SimpleDistribution.html)

Alternative source:
[Speeding up your website with Amazon CloudFront](https://docs.aws.amazon.com/AmazonS3/latest/userguide/website-hosting-cloudfront-walkthrough.html)

Amazon CloudFront improves the performance of the static website hosted on S3 by
making the website files (such as HTML, images, and video) available from data 
centers around the world

- Open the CloudFront console at https://console.aws.amazon.com/cloudfront/v3/home.

- Click Create Distribution

- On the Create Distribution page, in the Origin section, 
  - click the Origin domain textbox and the list of S3 buckets will appear. 
    Select the S3 website endpoint, for example, 
    `gt-simple-website01.s3.us-west-2.amazonaws.com`. The Name will be populated.
  - For the other settings, accept the default values.

- For the settings in the "Default Cache Behavior" section, accept the default 
  values. For more information about cache behavior options, see 
  [Cache behavior settings](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesCacheBehavior).

- For the settings under the "Settings" section, accept the default values.
  For more information about distribution options, see 
  [Distribution settings](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesGeneral).

- At the bottom of the page, choose Create Distribution.

- After CloudFront creates your distribution, the value of the Status column for
  your distribution changes from In Progress to Deployed. This typically takes
  a few minutes.

- Record the domain name that CloudFront assigns to your distribution, which 
  appears in the list of distributions. (It also appears on the General tab for
  a selected distribution.) It looks similar to the following: 
  `d111111abcdef8.cloudfront.net`.

- Open browser and go to `https://d111111abcdef8.cloudfront.net/index.html`.
  NOTE: it should work for both: HTTP and HTTPS

### [Render.com](Render.com)

- Register an account with [Render.com](https://render.com/)

- Render documentation for [Static Sites](https://render.com/docs/static-sites)

- Deployment by linking this Github repo to Render

- Note: Static sites on Render are free, with no cost at all unless the
  traffics go above 100 GB of bandwidth per month.
  
- **CLEANUP** - remember to delete the website using Render web console.


## References

- [Quick and easy HTTP servers for local development](https://timnwells.medium.com/quick-and-easy-http-servers-for-local-development-7a7df5ac25ff)

- [Render - Static Sites](https://render.com/docs/static-sites)