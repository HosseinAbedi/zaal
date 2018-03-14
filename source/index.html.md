---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='http://miras-tech.com/'>Develped at Miras</a>

<!--includes:
  - errors-->

search: true
---

# Introduction

```ruby
require 'uri'
require 'net/http'

url = URI("http://localhost:6666/Zaal/version")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Get.new(url)

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://localhost:6666/SnowWhite/version"

response = requests.request("GET", url)

print(response.text)
```

```shell
curl --request GET \
  --url http://localhost:6666/SnowWhite/version
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:6666/SnowWhite/version",
  "method": "GET",
  "headers": {}
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```
> The above command returns JSON structured like this:

```json
{
    "message": "Hey you! Welcome to Zaal's API! You can use our API to access Zaal API endpoints. Zaal is a Natural Language Processing (NLP)  service for real world applciations. The first NLP service for Persian Laguage.",
    "name": "Zaal",
    "version": "0.0.1"
}
```

Welcome to Zaal API! You can use our API to access Zaal API endpoints.
Zaal is a Natural Language Processing (NLP) service for real world applciations. The first NLP service for Persian Laguage. 

Persian language could be considered a major langauge in the family of Indo-Eaurpean languages where its native speakers and people who speak Persian as a second lanaguge add up to 110M people. Despite the vast population of Persian speakers there are not many resources for processing Persian documents in the field of Natural Language Processing (NLP). With this incentive we at Miras decided to start a project aiming at provinding an NLP web service with real world applicatons. 

Zaal currently supprots the following tasks:
<ul>
  <li>Document Classifier</li>
  <li>Document Level Sentiment Analysis</li>
  <li>Aspect Based Sentiment Analysis</li>
  <li>Similar Word Suggester</li>
  <li>Keyword Extractor</li>
</ul>

And we plan to support these tasks in the upcoming version:
<ul>
  <li>Event Extractor</li>
  <li>Named Entity Recognizer</li>
  <li>Summarizer</li>
</ul>

# RESTful API

## General Format

Apart from the request for getting the version of the service all other methods are of type POST request.

### HTTP Request

```{
‫‪"method":‬‬ ‫‪METHOD,‬‬  ‫‪"args": ARGUMENTS‬‬
}```

The end points for Zaal Service are as follows:

* `http://localhost:6666/SnowWhite/version`
* `http://localhost:6666/SnowWhite/api`


### Query Parameters

Parameter | Description
--------- | -----------
‪METHOD | The method used for the given arguments.
ARGUMENTS | Inputs for METHOD.

## Document Classifier

```ruby
require 'uri'
require 'net/http'

url = URI("http://localhost:9091/Zaal/api")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n   \"method\":\"document_classifier\",\n   \"args\":{\n      \"document\":\"رهبر انقلاب در دیدار با اعضایشورای عالی انقلاب فرهنگی، بر ضرورت تبیین مسیر دقیق فرهتگ تاکید کردند \"\n   }\n}"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://localhost:9091/Zaal/api"

payload = "{\n   \"method\":\"document_classifier\",\n   \"args\":{\n      \"document\":\"رهبر انقلاب در دیدار با اعضایشورای عالی انقلاب فرهنگی، بر ضرورت تبیین مسیر دقیق فرهتگ تاکید کردند \"\n   }\n}"
headers = {'Content-Type': 'application/json'}

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```

```shell
curl --request POST \
  --url http://localhost:9091/Zaal/api \
  --header 'Content-Type: application/json' \
  --data '{
   "method":"document_classifier",
   "args":{
      "document":"رهبر انقلاب در دیدار با اعضایشورای عالی انقلاب فرهنگی، بر ضرورت تبیین مسیر دقیق فرهتگ تاکید کردند "
   }
}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:9091/Zaal/api",
  "method": "POST",
  "headers": {
    "Content-Type": "application/json"
  },
  "processData": false,
  "data": "{\n   \"method\":\"document_classifier\",\n   \"args\":{\n      \"document\":\"رهبر انقلاب در دیدار با اعضایشورای عالی انقلاب فرهنگی، بر ضرورت تبیین مسیر دقیق فرهتگ تاکید کردند \"\n   }\n}"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "results": {
        "pay_load": {
            "Culture_Art": 0,
            "Politics_Internal": 0.1203,
            "Sports": 0.0078,
            "Entertainment": 0,
            "Social_Security": 0,
            "Social_Family": 0,
            "Industry_Banking": 0,
            "Culture_Tourism": 0,
            "Culture_Religion": 0.4811,
            "Economy": 0,
            "Industry_Technology": 0,
            "Industry_Production": 0,
            "Industry_Transportation": 0,
            "Industry_Real State": 0,
            "Social_Education": 0,
            "Industry_Automoblie Manufacturing": 0,
            "Industry_Insurance": 0,
            "Politics_Foreign": 0.0697,
            "Social_Urban": 0,
            "Medical": 0
        },
        "status": "success"
    }
}
```

A document as a in news articles can be classified based the different topics it addresses. In Zaal we use a multi-label classifcation where each document can be assinged a set of problaistic scores representing the possiblities of belonging to various classes.

Supported classes are:
<table class="table table-striped table-bordered" style="margin-left: auto; margin-right: auto;">
<tbody>
<tr>
<th style="text-align: center;">Industry</th>
<th style="text-align: center;">Politics</th>
<th style="text-align: center;">Culture</th>
<th style="text-align: center;">Social</th>
<th style="text-align: center;">Sports</th>
<th style="text-align: center;">Entertainment</th>
<th style="text-align: center;">Economy</th>
<th style="text-align: center;">Medical</th>
</tr>
<tr style="text-align: center;">
<td>Insurance</td>
<td>Internal</td>
<td>Tourism</td>
<td>Education</td>
<td>Sports</td>
<td>Entertainment</td>
<td>Economy</td>
<td>Medical</td>
</tr>
<tr style="text-align: center;">
<td>Transportation</td>
<td>Foreign</td>
<td>Art</td>
<td>Urban</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
<tr style="text-align: center;">
<td>Production</td>
<td>&nbsp;</td>
<td>Religion</td>
<td>Security</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
<tr style="text-align: center;">
<td>Automoblie Manufacturing</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>Family</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
<tr style="text-align: center;">
<td>Banking</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
<tr style="text-align: center;">
<td>Real State</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
<tr style="text-align: center;">
<td>Technology</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
</tbody>
</table>

We are working on a mechanism for the user to define custom classes which will be released in the upcomming version.

## Ducument Level Sentiment Analysis

```ruby
require 'uri'
require 'net/http'

url = URI("http://89.165.11.238:9091/Zaal/api")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n   \"method\":\"sentiment_analyzer\",\n   \"args\":{\n      \"document\":\" گوشی بدرد نخوریه \"\n   }\n}"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://89.165.11.238:9091/Zaal/api"

payload = "{\n   \"method\":\"sentiment_analyzer\",\n   \"args\":{\n      \"document\":\" گوشی بدرد نخوریه \"\n   }\n}"
headers = {'Content-Type': 'application/json'}

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```

```shell
curl --request POST \
  --url http://89.165.11.238:9091/Zaal/api \
  --header 'Content-Type: application/json' \
  --data '{
   "method":"sentiment_analyzer",
   "args":{
      "document":" گوشی بدرد نخوریه "
   }}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://89.165.11.238:9091/Zaal/api",
  "method": "POST",
  "headers": {
    "Content-Type": "application/json"
  },
  "processData": false,
  "data": "{\n   \"method\":\"sentiment_analyzer\",\n   \"args\":{\n      \"document\":\" گوشی بدرد نخوریه \"\n   }\n}"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "results": {
        "status": "success",
        "pay_load": {
            "sentiment_score": -0.9196
        }
    }
}
```

This a method for labeling documents with a seintiment score. Given a document in Persian this method returns a score in [-1, +1] where +1 represents a highly postive opinion and -1 designates a high degree of negativity in the document.

For example given ```گوشی بدرد نخوریه``` as the input document, this method assings a sentiment score of -0.9196 to it.

## Aspect Based Sentiment Analyzer


```ruby
require 'uri'
require 'net/http'

url = URI("http://localhost:9091/Zaal/api")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n   \"method\":\"aspect_sentiment_analyzer\",\n   \"args\":{\n      \"document\":\" دوربین خیلی خوبی است. تو خریدش شک نکنید \",\n      \"aspect\":\"دوربین\"\n   }\n}"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://localhost:9091/Zaal/api"

payload = "{\n   \"method\":\"aspect_sentiment_analyzer\",\n   \"args\":{\n      \"document\":\" دوربین خیلی خوبی است. تو خریدش شک نکنید \",\n      \"aspect\":\"دوربین\"\n   }\n}"
headers = {'Content-Type': 'application/json'}

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```

```shell
curl --request POST \
  --url http://localhost:9091/Zaal/api \
  --header 'Content-Type: application/json' \
  --data '{
   "method":"aspect_sentiment_analyzer",
   "args":{
      "document":" دوربین خیلی خوبی است. تو خریدش شک نکنید ",
      "aspect":"دوربین"
   }
}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:9091/Zaal/api",
  "method": "POST",
  "headers": {
    "Content-Type": "application/json"
  },
  "processData": false,
  "data": "{\n   \"method\":\"aspect_sentiment_analyzer\",\n   \"args\":{\n      \"document\":\" دوربین خیلی خوبی است. تو خریدش شک نکنید \",\n      \"aspect\":\"دوربین\"\n   }\n}"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "results": {
        "status": "success",
        "pay_load": {
            "sentiment_score": 0.9567
        }
    }
}
```

There could be mutiple segments of a document where each segment has its own sentiment score. This happens  whenever there are various aspects of an entity or a concept in a document. For example a comment about a new Samsung cellphone could contain a compelment about its colour and also some complaints about its high price.  


## Word Suggester

```ruby
require 'uri'
require 'net/http'

url = URI("http://localhost:9091/Zaal/api")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n   \"method\":\"word_suggester\",\n   \"args\":{\n      \"word\":\"ایرانی\",\n      \"k-nearest\":10\n   }\n}"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://localhost:9091/Zaal/api"

payload = "{\n   \"method\":\"word_suggester\",\n   \"args\":{\n      \"word\":\"ایرانی\",\n      \"k-nearest\":10\n   }\n}"
headers = {'Content-Type': 'application/json'}

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```

```shell
curl --request POST \
  --url http://localhost:9091/Zaal/api \
  --header 'Content-Type: application/json' \
  --data '{
   "method":"word_suggester",
   "args":{
      "word":"ایرانی",
      "k-nearest":10
   }
}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:9091/Zaal/api",
  "method": "POST",
  "headers": {
    "Content-Type": "application/json"
  },
  "processData": false,
  "data": "{\n   \"method\":\"word_suggester\",\n   \"args\":{\n      \"word\":\"ایرانی\",\n      \"k-nearest\":10\n   }\n}"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "results": {
        "pay_load": {
            "آمریکایی": 0.61,
            "امریکایی": 0.5608,
            "ایران": 0.592,
            "ژاپنی": 0.6134,
            "کشورمان": 0.5965,
            "ایرانی": 1,
            "هندی": 0.5863,
            "خارجی": 0.5611,
            "چینی": 0.5934,
            "روسی": 0.558
        },
        "status": "success"
    }
}
```

One application of NLP is to extract synonyms of a word. Using this method you could get the k-nearest words similar to your word. 

## Keyword Extractor

```ruby
require 'uri'
require 'net/http'

url = URI("http://localhost:9091/Zaal/api")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n   \"method\":\"keyword_extractor\",\n   \"args\":{\n      \"document\":\"رهبر انقلاب در دیدار با اعضایشورای عالی انقلاب فرهنگی، بر ضرورت تبیین مسیر دقیق فرهتگ تاکید کردند \"\n   }\n}"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://localhost:9091/Zaal/api"

payload = "{\n   \"method\":\"keyword_extractor\",\n   \"args\":{\n      \"document\":\"رهبر انقلاب در دیدار با اعضایشورای عالی انقلاب فرهنگی، بر ضرورت تبیین مسیر دقیق فرهتگ تاکید کردند \"\n   }\n}"
headers = {'Content-Type': 'application/json'}

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```

```shell
curl --request POST \
  --url http://localhost:9091/Zaal/api \
  --header 'Content-Type: application/json' \
  --data '{
   "method":"keyword_extractor",
   "args":{
      "document":"رهبر انقلاب در دیدار با اعضایشورای عالی انقلاب فرهنگی، بر ضرورت تبیین مسیر دقیق فرهتگ تاکید کردند "
   }
}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:9091/Zaal/api",
  "method": "POST",
  "headers": {
    "Content-Type": "application/json"
  },
  "processData": false,
  "data": "{\n   \"method\":\"keyword_extractor\",\n   \"args\":{\n      \"document\":\"رهبر انقلاب در دیدار با اعضایشورای عالی انقلاب فرهنگی، بر ضرورت تبیین مسیر دقیق فرهتگ تاکید کردند \"\n   }\n}"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "results": {
        "pay_load": {
            "انقلاب": 0.0191,
            "شورای": 0.0084,
        },
        "status": "success"
    }
}
```

Keyword extractor (as the name suggests) extracts the most important words from a given text. This method extracts keywords and an importance score associated with each keyword from text.

