

安裝Gitlab



https://about.gitlab.com/downloads/\#ubuntu1604

裝好預設是80port，可以如下更改port



1. `sudo -e /etc/gitlab/gitlab.rb`
2. Change
   external\_url
   from
   `yourdomain.com`
   to
   `yourdomain.com:9999`
3. `sudo gitlab-ctl reconfigure`

  


