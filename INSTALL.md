## Cloud Foundry Elastic Search Installation

To deploy Elastic Search on Cloud Foundry

- Clone this repo OR
  - Download the latest source from http://www.elasticsearch.org
  - Extract the zip
  - Copy `config/elasticsearch.yml` and `manifest.yml` to the extracted dir
- Install vmc `gem install vmc --pre` (Standalone apps require version >= vmc 0.3.16.beta.4)
- Log into a Cloud Foundry instance that has standalone apps enabled
- Push your app
 
``` bash
  $ vmc push
    Would you like to deploy from the current directory? [Yn]: Y
    Pushing application 'search-95c3e'...
    Creating Application: OK
    Uploading Application:
    Checking for available resources: OK
    Processing resources: OK
    Packing application: OK
    Uploading (2K): OK
    Push Status: OK
    Staging Application 'search-95c3e': OK
    Starting Application 'search-95c3e': OK

  $ vmc apps

  +--------------+----+---------+-------------------------------------------+----------+
  | Application  | #  | Health  | URLS                                      | Services |
  +--------------+----+---------+-------------------------------------------+----------+
  | search-95c3e | 1  | RUNNING | search-95c3e.appcloud20.dev.mozycloud.com |          |
  +--------------+----+---------+-------------------------------------------+----------+
  
```

Manifest will take care of generating a unique name for your app.
And you are ready to use elastic search over HTTP at the url generated.