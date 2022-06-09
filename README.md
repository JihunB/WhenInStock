# WhenInStock

![Build](https://github.com/EricJMarti/inventory-hunter/workflows/Build/badge.svg) ![Docker Pulls](https://img.shields.io/docker/pulls/ericjmarti/inventory-hunter) ![Docker Image Size (tag)](https://img.shields.io/docker/image-size/ericjmarti/inventory-hunter/latest)

## Requirements

- Raspberry Pi 2 or newer (alternatively, you can use an always-on PC or Mac)
- [Docker](https://www.docker.com/) ([tutorial](https://phoenixnap.com/kb/docker-on-raspberry-pi))

You will also need one of the following:
- [Discord Webhook URL](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks)
- [Slack Webhook URL](https://api.slack.com/messaging/webhooks)
- [Telegram Webhook URL and Chat ID](https://core.telegram.org/bots/api)
- [SMTP relay to send automated emails](https://medium.com/swlh/setting-up-gmail-and-other-email-on-a-raspberry-pi-6f7e3ad3d0e)


## What does this project do?

As the name suggests, this Open-source Software notifies you when a high demand product you wanted gets back in stock below specific price.  
This script continually refreshes a set of URLs, looking for the "add to cart" phrase. Once detected, an automated alert is sent, giving you an opportunity to react.   
The notification may be sent through the method desired by the user (Discord, Slack, Telegram, email).   
  
<img width="468" alt="Picture1" src="https://user-images.githubusercontent.com/91535297/172469099-ea0b9a63-e8a6-4249-8cb4-58427ec0cad5.png">



## How to get started?

Please test on Raspberry Pi OS with Docker already installed.

1. Clone this repository and pull the latest image
    ```
    pi@raspberrypi:~
    $ git clone https://github.com/JihunB/WhenInStock

    pi@raspberrypi:~
    $ cd WhenInStock

    pi@raspberrypi:~/WhenInStock
    $ sudo docker pull ericjmarti/inventory-hunter:latest
    ```

2. Create your own configuration file based on one of the provided examples:

    - [Amazon RTX 3080 config](config/amazon_rtx_3080.yaml)
    - [Best Buy RTX 3060 Ti config](config/bestbuy_rtx_3060_ti.yaml)
    - [B&H Photo Video RTX 3070 config](config/bhphoto_rtx_3070.yaml)
    - [Micro Center RTX 3070 config](config/microcenter_rtx_3070.yaml)
    - [Newegg RTX 3070 config](config/newegg_rtx_3070.yaml)

3. Start the Docker container using the provided `docker_run.bash` script, specifying the required arguments.

    If using Discord or Slack, the format of your command will look like this:

    ```
    $ ./docker_run.bash -c <config_file> -a <discord_or_slack> -w <webhook_url>

    # Discord example:
    pi@raspberrypi:~/WhenInStock
    $ ./docker_run.bash -c ./config/newegg_rtx_3070.yaml -a discord -w https://discord.com/api/webhooks/...
    ```
    
    
    If using an SMTP relay, the format of your command will look like this:

    ```
    $ ./docker_run.bash -c <config_file> -e <email_address> -r <relay_ip_address>

    # SMTP example:
    pi@raspberrypi:~/WhenInStock
    $ ./docker_run.bash -c ./config/newegg_rtx_3070.yaml -e myemail@email.com -r 127.0.0.1
    ```

## How to stop and test other files?

1. First identify any running container names related to WhenInStock
    ```
    $ docker ps
    ```
2. Stop and remove all containers related to WhenInStock
    ```
    $ docker stop CONTAINER_NAME
    $ docker rm CONTAINER_NAME
    ```
3. Pull repo updates
    ```
    $ git pull
    ```
4. Rerun the docker_run.bash command to start containers back up with updates.
    ```
    $ ./docker_run.bash -c <config_file> -a <discord_or_slack> -w <webhook_url>
    ```

## Configuring Alerters

If you are interested in configuring multiple alerters or would like to keep your alerter settings saved in a file, you can configure WhenInStock's alerting mechanism using a config file similar to the existing scraper configs.

1. Create a file called alerters.yaml in the config directory.

2. Configure the alerters you would like to use based on this example:

    ```
    ---
    alerters:
      discord:
        webhook_url: https://discord.com/api/webhooks/XXXXXXXXXXXX...
        mentions:
          - XXXXXXXXXXXXXXX
          - XXXXXXXXXXXXXXX
      telegram:
        webhook_url: https://api.telegram.org/botXXXXXXXXXXXXXXXXXXXX/sendMessage
        chat_id: XXXXXXXX
      email:
        sender: myemail@email.com
        recipients:
          - myemail@email.com
          - myfriendsemail@email.com
        relay: 127.0.0.1
        password: XXXXXXXXXX   # optional
      slack:
        webhook_url: https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX
        mentions:
          - XXXXXXXXXXXXXXX
          - XXXXXXXXXXXXXXX
    ...
    ```

3. Add this config file to your run command:

    ```
    pi@raspberrypi:~/WhenInStock
    $ ./docker_run.bash -c ./config/newegg_rtx_3070.yaml -q ./config/alerters.yaml
    ```


## Why is this project useful?

- you can buy high demand products anytime with least effort
- it runs on your own hardware, so no processing time is spent servicing other users
- you get to choose which products you want to track
- you are in control of the refresh frequency

## Contribution

- included more links to high demand products and rearranged them in the order of links most likely to be found  
- due to the increased security of the Captcha, links that interrupt the 2-second refresh and slow down the program have been erased  
- when the average market price fell, the price sought for each product was lowered  
- the errors that occur frequently were organized and added to CheckError.txt file    

## Presentation Video (YouTube) Link

https://youtu.be/2vE4boTr7eA

## References

https://github.com/EricJMarti/inventory-hunter
https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

## Where can I get more help, if needed?

Feel free to contact me for any reason via email 👍 : 22100344@handong.ac.kr

