# first_hub
First repo
CADDY


A Docker image for Caddy.

Requirement is

    Pull the latest code from Caddy
    Build it
    Run the tests
    Write a Dockerfile
    Package it as a Docker image
    Run the dockerized caddy and deploy a hello-world web page
    Verify that Caddy serves the hello-world page properly
    If anything goes wrong stop and provide a report
    Add a detailed README file that explains your solution, how to use it, various decisions and any assumptions you made
    Submit a zip file with all the files (or read access to a private git repo)
    Important: Please don't publish your solution on a public git repo

I am using the source link "https://github.com/mholt/caddy" and building the Dockerfile

The Dir contains
Caddyfile  Dockerfile  index.html

Dockerfile is used to build the container
Caddyfile is config file to customize how your site is served
index.html is base file with Hello world

To build the image run the below command
docker build -t helix_caddyimage .

To run the container please run
docker run -d -p 80:80 -p 443:443 -p 2015:2015 --name helix_image helix_caddyimage

To hit the URL please use the HOST ipaddress.

Some of the commands used are explained/commented in the Dockerfile

used setcap cap_net_bind_service=+ep which tells, caddy is allowed to run as non-root user and use  port 80(http) and 443(https)
which are administraive ports.


Logs requests to an access log which is /var/log/access.log and errors to  /var/log/error.log
Proxies all API and adds the coveted Access-Control-Allow-Origin: * header for all responses from the API.

Also, i am not creating the SSL cert  but this can be implemented if you have  registered domain and can create the ssl cert using the free
letsencrypt certs.

Caddyfile has the following entries

       localhost {

  root /home/appuser/caddy/app
  log /var/log/caddy/access.log
  errors /var/log/caddy/errors.log
  gzip
  push
  browse
  ext    .html

proxy  /api 127.0.0.1:7005
header /api Access-Control-Allow-Origin *

}

when you hit the URL it  will serve using the https but with an unsigned cert.
