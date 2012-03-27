## Cloud Foundry Elastic Search Installation

To deploy Elastic Search on Cloud Foundry

- Clone this repo OR
  - Download the latest source from http://www.elasticsearch.org
  - Extract the zip
  - Copy `config/elasticsearch.yml` and `manifest.yml` to the extracted dir
- Install vmc `gem install vmc --pre` (Standalone apps require version >= vmc 0.3.16.beta.4)
- Log into a Cloud Foundry instance that has standalone apps enabled
- Push your app
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

Manifest will take care of generating a unique name for your app.
And you are ready to use elastic search over HTTP at the url generated.

## Testing from bash

``` bash

/* Put here the proper app url you generated  */
export APP_URL=search-95c3e.appcloud20.dev.mozycloud.com

/* Add data */
curl -XPUT http://$APP_URL/twitter/user/kimchy -d '{
    "name" : "Shay Banon"
}'

curl -XPUT http://$APP_URL/twitter/tweet/1 -d '{
    "user": "kimchy",
    "post_date": "2009-11-15T13:12:00",
    "message": "Trying out elasticsearch, so far so good?"
}'

curl -XPUT http://$APP_URL/twitter/tweet/2 -d '{
    "user": "kimchy",
    "post_date": "2009-11-15T14:12:12",
    "message": "You know, for Search"
}'

/* Get Data */
curl -XGET http://$APP_URL/twitter/tweet/2

/* Test Search */
curl -XGET http://$APP_URL/twitter/tweet/_search?q=user:kimchy

curl -XGET http://$APP_URL/twitter/tweet/_search -d '{
    "query" : {
        "term" : { "user": "kimchy" }
    }
}'

curl -XGET http://$APP_URL/twitter/_search?pretty=true -d '{
    "query" : {
        "range" : {
            "post_date" : {
                "from" : "2009-11-15T13:00:00",
                "to" : "2009-11-15T14:30:00"
            }
        }
    }
}'

```