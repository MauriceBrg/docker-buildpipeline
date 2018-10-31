# About

This image installs runs the latest LTS-Version of Jenkins, installs some plugins that are required in order to have a semi-usable interface. Furthermore docker is installed in order to be able to build docker stuff.

# Environment Variables
- Add `ADMIN_PASSWORD` as an environment variable. This should contain the password for the admin account.

# How to
1. Run the image, make sure to map `/var/run/docker.sock` as `/var/run/docker.sock` into the container
2. Watch the output or get the password for the installer from `/var/jenkins_home/secrets/initialAdminPassword` 
3. Visit the website, enter the password and skip as much of the setup as possible - the most important stuff is already handled.
4. Login and enjoy

# Notes
- The Jenkins User has Access to the Docker Host when mapping the socket into the container - this is dangerous! (But necessary in order to build containers)