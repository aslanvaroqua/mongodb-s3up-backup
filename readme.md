Took a script by RGB Boy that did not work for me out of the box and instead of using keys, doing manual md5/ signatures, I am using s3up... You configure once and your done. 
You dont need to put amazon keys and such in the file.. much much easier. You can also use this as the basis for other s3 related projects. s3up is much easier than manual calls. 

Instead of this:

HEADER_DATE=$(date -u "+%a, %d %b %Y %T %z")
CONTENT_MD5=$(openssl dgst -md5 -binary $DIR/backup/$ARCHIVE_NAME | openssl enc -base64)
CONTENT_MD5=""
CONTENT_TYPE="application/x-download"
STRING_TO_SIGN="PUT\n$CONTENT_MD5\n$CONTENT_TYPE\n$HEADER_DATE\n/$S3_BUCKET/$ARCHIVE_NAME"
SIGNATURE=$(echo -e -n $STRING_TO_SIGN | openssl dgst -sha1 -binary -hmac $AWS_SECRET_KEY | openssl enc -base64)

You can just do this:

s3cmd put $ARCHIVE_NAME s3://$S3_BUCKET/$ARCHIVE_NAME

and instead of this:
sh ./backup.sh -p test -k {ACESSKEY8234983294324}  -s {SECRETKEY2394820394832}  -b cds-backup-analytics -r us-west-2 

this:
sh ./backup.sh -p mongopass -u mongouser  -b cds-backup-analytics 

Much easier :). 

## Requirements

* s3up
* Running mongod process
* mongodump
* mongo
* openssl
* tar
* rm
* curl

# Install s3 up and configure first.
sudo apt-get install s3up

s3up --configure

## Usage

`bash /path/to/backup.sh -u MONGODB_USER -p MONGODB_PASSWORD -b S3_BUCKET


## Cron

### Daily

Add the following line to `/etc/cron.d/db-backup` to run the script every day at midnight (UTC time) 

    0 0 * * * root /bin/bash /path/to/backup.sh -u MONGODB_USER -p MONGODB_PASSWORD -b S3_BUCKET

Aslan Varoqua

# License 

CREDIT TO RGBBOY FOR ORIGINAL SCRIPT  

(The MIT License)

Copyright (c) 2014 RGBboy &lt;l-_-l@rgbboy.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
