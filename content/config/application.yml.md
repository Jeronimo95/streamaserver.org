---
title: application.yml
description: Configuring Streama with the application.yml
date: 2018-10-06
publishdate: 2018-10-06
lastmod: 2018-10-06
menu:
  docs:
    parent: "config"
    weight: 20
weight: 20
sections_weight: 20
draft: false
toc: true
categories: [configuration]
---

Some settings need to be configured in the application.yml. If desired, create a file called application.yml or 
[download the sample here](https://github.com/streamaserver/streama/blob/master/docs/sample_application.yml) 
and place it next to the streama.jar file. When starting the application, this file (if named & placed correctly) 
will be automatically loaded.

With the application.yml you can do the following adjustments: 
- change the port of the application
- change the datasource from h2 to mysql and customize the authorization for either
- adjust the regex for the bulk-creation of movies & shows
- add googleAnalytics support
- the maxFileSize & maxRequestSize


# Sample application.yml:

```
environments:
    production:
        dataSource:
            #Default embedded database
            driverClassName:  'org.h2.Driver'
            url: jdbc:h2:./streama;MVCC=TRUE;LOCK_TIMEOUT=10000;DB_CLOSE_ON_EXIT=FALSE
            username: root
            password:
            
            #For mysql database
            #driverClassName:  'com.mysql.jdbc.Driver'
            #url: jdbc:mysql://localhost/streama
            #username: root
            #password:
        server:
            port: 8080

streama:
  regex:
    movies: ^(?<Name>.*)[._ ]\(\d{4}\).*
    shows:
      - ^(?<Name>.+)[._ ]S(?<Season>\d{2})E(?<Episode>\d{2,3}).*
      - ^(?<Name>.+)[._ ](?<Season>\d{1,2})x(?<Episode>\d{2,3}).*
      
  googleAnalytics:
      enabled: true
      id: 'UA-11111111-1'
```

# How to use this page

In the below list of settings settings are listed like `setting0.setting1.setting2: value` in the application.yml this would be formatted as:

```
setting0:
    setting1:
        setting2: value
```

If there are multiple ones they can be merged, for example the following setting and the one above would be: `setting0.setting1.othersetting: value`

```
setting0:
    setting1:
        setting2: value
        othersetting: value
```


# application.yml options:
This is not an exhaustive list, options from grails / springboot can be used.


### environments.production.dataSource
See [databases](/config/databases) on how to configure data sources.
```
environments:
    production:
        dataSource:
            driverClassName:  'com.mysql.jdbc.Driver'
            url: jdbc:mysql://localhost/streama
            username: root
            password:
```



### server.port
The port to run the server on. You cannot use values below 1024 unless you are root **and it is *not* recommended to run Streama as root.**
If you want to run on port 80 or with HTTPS see [Reverse proxies](/config/proxy)
```
environments:
  production:
    server:
      port: 8081
```


### streama.regex.movies & streama.regex.shows
The regular expression to use for matching movies & TV-Shows when bulk-adding. Can also be a list of multiple RegExs.
```
streama:
  regex:
    movies: ^(?<Name>.*)[._ ]\(\d{4}\).*
    shows:
      - ^(?<Name>.+)[._ ]S(?<Season>\d{2})E(?<Episode>\d{2,3}).*
      - ^(?<Name>.+)[._ ](?<Season>\d{1,2})x(?<Episode>\d{2,3}).*
```

### grails.controllers.upload.maxFileSize & maxRequestSize
The max file size that can be uploaded & requested. Default to 10TB.

```
grails:
  controllers:
     upload:
      	maxFileSize: 10TB
      	maxRequestSize: 10TB
```

### streama.googleAnalytics
Choose to enable googleAnalytics and pass in your GA ID (by default, this is disabled)
```
streama:
  googleAnalytics:
      enabled: true
      id: 'UA-11111111-1'
```
