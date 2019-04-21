# Tips/Solutions for Running Zabbix and integrating Services

## Tips for Zabbix
---
1. Use Ubuntu image tag for docker-compose image of zabbix-server-pgsql.
2. Keep default ports for application, or you will have blocked port or connection refused issues.
3. If you have binding issue then , change first external port only like
  `8090:80` if u having issue with `8080:80`.  

## Configuring Action for Alerts based on Triggers
***

1. Go to Configuration > Actions.
2. Create Action with Triggers selected.
3. Under *Action* tab
  - Name - Name of action you want
  - Conditions - All conditions on which action will be called are shown here
  - New condition - This you can use to add trigger based on
     - Triggere name like - this can used to match trigger names/condition like `Processor load is too high on {HOST.NAME}`
     - Trigger Severity - Warning, information, average, high, disaster type
     - 
  - Enabled - Check this to enable trigger
4. Under *Operation* Tab
  - Operations > New . Then send to users and select user.
  - Send only to choose > media type. Then add to save.
5. Now the action has been configured.


## Creating a new Media type
***
Media are the resources by which we communicate and send our message like mail, sms or any chat.

1. Go to Administration > Media Types. Then create media type.
2. In Name field enter name of your media like gmail or slack.
3. Now choose type like mail, script.
4. For further set up read below guide for gmail and slack. 

## Connecting with Gmail for sending Alerts 
***

1. First Create a media with Gmail configuration.
2. Go to Administration > Media types.
3. Click on Email.
4. Now fill specific options like
 - **Name** - Anything you like to name your media Type like *Email* or *Gmail*.
 - **Type** - Type of media type to configure, choose here *Email*
 - **SMTP Server** - SMTP Domain of Gmail , write `smtp.gmail.com`
 - **SMTP Helo** - gmail.com   # for helo protocol communication
 - **SMTP Server Port** Port for connecting to Gmail SMTP service(Security connection) 
   - If you want to use None encryption, you need to set smtp port to 25
   - If you want to use STARTTLS, you need to set smtp port to 587
   - If you want to use SSL, you need to set smtp port to 465
 - **Connection Security** Depend on port you choose above  (Use 587 if u don't have ssl else use 465 with ssl for google)
   - **None** for port 25
   - **STARTTLS** for port 587
   - **SSL/TLS** for port 465 
 - **SSL verify Peer** - Untick 
 - **SSL verify Host** - Untick
 - **Authentication** - Username And Password for your Gmail Account
 - **Enabled** - Tick to Enable
5. Now click on Update to save.
6. Now link this media type to a user or user group.
7. For User, Go to Administration > Users > Admin > Media Tab (I have used Admin for example, you can use any)
8. Click on Add > then choose type *Email*
9. In *Send To* option write the email for user 
10. Check *Use if Severity* for setting warning/danger level.
11. Check *Enabled* for enabling email media for user.
12. Now click on *Add* to save.

## Connecting with Slack for Alerts over channel.
---

1. We will use this shell script from https://github.com/ericoc/zabbix-slack-alertscript. Here i have added to Zabbix-scripts also.
2. Mount volume for *AlertScripts* in both host and container as:
 ```
 zabbix-server:
     image: zabbix/zabbix-server-pgsql:ubuntu-latest
     volumes:
      - /usr/lib/zabbix/alertscripts:/usr/lib/zabbix/alertscripts 
 ```
3. Copy this Script to *AlertScripts* directory defined in your Docker-compose volumes mounted in your host.
By default `/usr/lib/zabbix/alertscripts/slack.sh` is the mount volume for container in docker-compose. You can also change if you want, just replace with your directory then.
4. Make sure the file is executable in container by command 
 ```
 ls -lha /usr/lib/zabbix/alertscripts/slack.sh
 Output :
 -rwxr-xr-x 1 root root 1.7K Jun 25 10:37 /usr/lib/zabbix/alertscripts/slack.shh

 For making executable run
 chmod +x /usr/lib/zabbix/alertscripts/slack.sh 
 ```
5. Make sure 'curl package is installed', if not install by commands (Make sure image is zabbix/zabbix-server-pgsql:ubuntu-latest)
```
sudo docker exec -it zabbix-new_zabbix-server_1 bash #replace container name with your container
apt update
apt install curl
```
6. We need to change Webhook URL with our custom url in slack script and username with what you want from the message to be appeared. Check How to create a webhook section below.
7. Now go to Administration > Media types > add .
8. Choose script type and enter
 - *Name* - Name of media type
 - *Type* - Choose script as type
 - *Script Name* - enter here full name of script, like *slack.sh*
 - *Script Parameters* - Additional parameters and variables for scripts. Enter below three 
   - {ALERT.SENDTO} 
   - {ALERT.SUBJECT}
   - {ALERT.MESSAGE} 
9. Tick *Enabled* for enabling this media type.
10. Now click on update for saving Slack media type.
11. Later steps are same as Gmail steps from 6th step, you just need to enter *#slackchannel* in place of *Send to*.

## Creating a Slack Webhook URL
---

1. Go to this url https://my.slack.com/services/new/incoming-webhook and create a new webhook.
2. Start by choosing a channel where your incoming webhook will post messages to like *general*.
3. Then click on creating a WebHook Url option to create it.
4. go below and copy Webhook Url you need.
5. Set Descriptive Label like *Integration with some service*
6. Customize name related to your purpose or anything like *new-webhook*.
7. Customize icon, you can choose from slack icons or upload new.
8. Now Save Settings to save.


