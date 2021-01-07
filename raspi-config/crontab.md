## shuthdown -h now

[automatic-shutdown-at-specified-times](https://askubuntu.com/questions/567955/automatic-shutdown-at-specified-times)
````
30 23 * * * root shutdown -h now
At 23:30 (11:30 PM), the kiosk will shut down. No matter what user is logged in, the shutdown command runs as root.
````
````
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command

# m h  dom mon dow   command
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                       7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * *  command to execute
*/30  * * * *  rsync -avz -e ssh root@192.168.2.90:/srv/www/htdocs/site/dokuwiki/ /home/pi/dokuwiki_rsync
@reboot sudo swapoff -a
30 18  * * * shutdown -h now
0 4 * * 1  sudo apt update && sudo apt upgrade -y
0 5 * * 1  rsync -Aax /home/pi/dokuwiki_rsync/ /home/pi/dokuwiki_rsync-dirbkp_`date +"%Y%m%d"`
# cp -r * /home/pi/dokuwiki/config/dokuwiki/conf
# @reboot cd /home/pi/nextcloud && docker-compose -t 10 restart



````
