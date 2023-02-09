# OpenHAB Matrix Integration
This describes how to send end to end encrypted messages via matrix protocol from openhab in docker by using matrix-commander in docker and named pipes. The description assumes you are using debian or similar as host system.

## General Functional Flow
Using a proxy string item to publish the message from any rule. Using a dedicate rule to read the changes to the proxy string item and call matrix-commander running in another docker container by using a named pipe mounted to the openhab docker container.

## Step 0: Consider Your Setup
Remember to check all paths in the following description to map your setup.

## Step 1: Set Up Named Pipe
For more details regadring the named pipes, refer to [this stackoverflow thread](https://stackoverflow.com/questions/32163955/how-to-run-shell-script-on-host-from-docker-container). Create a file under `/home/user/mypipe` on the host system.
Copy the cronjob script from `cron/execpipe.sh` in this repository to `/home/user/cron` on the docker host and create a cronjob, refer to crontab file in this repository. Reboot the docker host. Add the file `/home/user/mypipe` to your openhab container as volumen at the mount point `/openhab/hostpipe`, refer to minimal example in `docker-compose.yml`.

## Step 2: Pull matrix-commander Image
Make sure you have the matrix-commander image pulled by `docker pull matrixcommander/matrix-commander:6`

## Step 3: Set Up matrix commander
Make sure you have set up matrix-commander correctly for the rooms you want to send messages to. For details refer to [the matrix-commander github page](hhttps://github.com/8go/matrix-commander). Make sure you have your configuration data of matrix-commander (credentials file and stroge folder) ready at `/home/user/matrixcommander` on the docker host.

## Step 4: Create Proxy Items and Rules
Create string proxy items in openhab and proxy rules which call matrix-commander via the named pipe. Refer to examples in `openhab/conf/rules and openhab/conf/items` in this repro.

## Step 5: Test the Set-Up
For example, refer to test.rules in this repository at `openhab/conf/rules`.

