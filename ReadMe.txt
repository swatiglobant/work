Sophos-backendapp Deployment Process
===================

### Prerequisites
> - CentOS release 6.3
> - Ruby 2.2.5
> - Rails 4.1.5
> - Mysql 5.1.73
> - nginx 1.6.1
>- Phusion Passenger 4.0.50
>- ffmpeg 2.8.6
>- ImageMagick 6.9.3


### Preparing The Deployment Server

>- Preparing The Operating System (CentOS release 6.3)
>- Setting Up Ruby Environment and Rails (Ruby 2.2.5 & Rails 4.1.5)
>- Installing Mysql (Mysql 5.1.73)
>- Installing App. & HTTP Servers (Phusion Passenger 4.0.50)
>- Configuring Nginx For Application Deployment (nginx 1.6.1 )


#### **In case of fresh deployments(else ignore):**
Execute
```sh
RAILS_ENV=production rake db:setup 
```

----------

#### **Execute following commands(With Environment) after each deployments:**

1.**Rake Commands:**
```sh
bundle install
RAILS_ENV=production rake db:migrate
RAILS_ENV=production rake assets:clean
RAILS_ENV=production rake assets:precompile
```
2.**Initiate RPUSH:**

**rpush gem** ('2.2.0') : send APNS & GCM notifications

Use 	following command to initiate rpush

`rpush start -e production`


> **Note:**

>- It stores the pid file into the <Rails root>/tmp/pids/rpush.pid
>- Before executing this command, kill/stop already running rpush process.
To stop use
 `rpush stop -e production.`
 
>- In case you are replacing old release with new release folder, you have to use kill command to stop rpush

3.**Run “whenever gem” to run cron jobs:**
> clear the old cron jobs 	using 
`crontab -r`
>
Then execute
>
`bundle exec whenever --update-crontab sophos_production --set environment=production`

**Execute Following command only for this version:**

 
```sh
N/A
```


----------
### **General Comments:**
 
**Amazon Simple Storage Service (s3) configuration:**
>- IAM roles have been implemented to upload files on S3,Bucket name should be available in  **environment variables**.
>- **Refer:** *config/config.yml* file for environment variable's name


**Gem installation**
>Use Gemfile or Gemfile.lock file to map gem's versions