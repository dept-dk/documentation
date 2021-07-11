# Keyword Research Tool Documentaions
## Unlisted & Test & Public Version of KRT (Code repository, Development, Deployment, etc.)
### Info
- Unlisted Version
  - Apps Script (Code): https://script.google.com/a/deptagency.com/d/1a7NzRQoh8-jie476RAoxX7ewqSw9J3y4qDnkLz4S6VPntTcurpEww5fx
  - Marketplace: https://console.cloud.google.com/apis/api/appsmarket-component.googleapis.com/googleapps_sdk?project=project-id-5455635496199677748
- Test Version
  - Apps Script (Code): https://script.google.com/a/deptagency.com/d/1ASEcbMoimWtu14D2FB4zZjXgUSDjqMt3qtqg5UFLCpMWiBNW-mAq5leD/
  - Marketplace: https://console.cloud.google.com/apis/api/appsmarket-component.googleapis.com/googleapps_sdk?project=keyword-research-test-254809&folder=&organizationId=
  - 
- Public Version
  - Apps Script (Code): https://script.google.com/a/deptagency.com/d/1vqrk-nEfuYTepqmheTd1ucusQWojRN_RqKTqmozBqOX5chmm3lC0qOJ2/
  - Marketplace:
  https://console.cloud.google.com/apis/api/appsmarket-component.googleapis.com/googleapps_sdk?project=dept-keyword-research

### Develop
- Editors
  - Online: Open the Apps Script Project, there are two types of online editor(legacy and new)
  - Local (like VS Code)
> To develop and manage Apps Script projects from your terminal rather than the Apps Script editor, you can use an open-source tool called clasp.
> 
> Clasp lets you to develop your Apps Script projects locally. You can write code on your own computer and upload it to Apps Script when you're done. You can also download existing Apps Script projects so that you can edit them when you're offline. Since the code is local, you can use your favorite development tools like git when building Apps Script projects.

- Clasp
  - Requirements
    - Npm
    - Node.js 4.7.4
  - Installation
    - `npm install @google/clasp -g`
  - Using clasp
    - Login `clasp login`
    - Logout `clasp logout`
    - Clone an existing project `clasp clone {Script ID}`
    - Download a script project `clasp pull`
    - Upload a script project `clasp push`
    - List project versions `clasp versions`
  
  Check the links below for more details <br>
  - https://developers.google.com/apps-script/guides/clasp
  - https://codelabs.developers.google.com/codelabs/clasp
  - https://www.youtube.com/watch?v=lwxiEB-Mnys

### Test
(Legacy editor) Run -> Test as add-on -> Execute Saved Test (Configure new test first If there are no documents that you want to test).
### Deploy/Update
1. Save new version (Open Apps Script -> File -> Manage versions)
2. Open **Google Workspace Marketplace SDK** in Google Cloud Platform, then **App Configuration**
3. Input the version number from the first step and save


## Getting Google Search Volume & Search Console data
There are two services on the server to provide search volume and search console query funcions for KRT. They are based on the **Google Ads API** and **Search Console API**

Guides for Google Ads API: https://developers.google.com/google-ads/api/docs/start

Guides for Search Console API: https://developers.google.com/webmaster-tools/search-console-api-original/v3/prereqs

### Google Search Volume
- API access using own credentials (installed application flow)
https://github.com/googleads/googleads-php-lib/wiki/API-access-using-own-credentials-(installed-application-flow)
## Login Service
This service is used for switching accounts function of retrieving search console data.
- Code: {GoogleSearchConsole}/Api/
- oauth2callback
- getuser
## BigQuery API Integration (config, API design) 

### Documentation
https://googleapis.dev/nodejs/bigquery/latest/index.html
### Code
https://github.com/dept-dk/server

### API
- [POST] /bigquery/dataset: 
  - function: create a new dataset
  - parameters: {name: string}
  - 
  ```json
  {
	  "name": "jysk"
  }
  ```
  - response
  ```json
  {
    "status": 0, //success, 1 if fail
    "message": {
      "id": "kwr-data:jysk"
    }
  }
  ```
- [DELETE] /bigquery/dataset\
  - function: delete a dataset
  - parameters: {id}
  - 
  ```json
  {
	  "id": "jysk"
  }
  ```
  - response
  ```json
  {
    "status": 1,//fail, 0 if success
    "message": {
      "code": 404,
      "errors": [{
      "message": "Not found: Dataset kwr-data:jysk"
    }]
  }
  ```
- [POST] /bigquery/table
  - create a new table
  - parameters: {datasetId, name, schema}
  - 
  ```json
  {
    "datasetId": "JYSK",
    "name": "Baltic_Belarus_Categorizer_20210607",
    "schema": [
      {"name":"search_term","type":"STRING"},
      {"name":"volume","type":"INTEGER"},
      {"name":"group","type":"STRING"},
      {"name":"date","type":"TIMESTAMP"}
    ]
  }
  ```
  - response
  ```json
  {
    "status": 0,
    "message": {
      "id": "Baltic_Belarus_Categorizer_20210607"
    }
  }
  ```
- [POST] /bigquery/data
  - upload data to the BigQuery
  - parameters: {datasetId, tableId, data}
  - 
  ```json
  {
    "datasetId": "JYSK",
    "tableId": "Baltic_Belarus_Categorizer_20210607",
    "data": [
      {
        "search_term": "vacuasticbag",
        "volume": 90,
        "group": null
      },
      {
        "search_term": "velicebag",
        "volume": 10,
        "group": null,
        "date": 1622100194
      },
      {
        "search_term": "16",
        "volume": 14800,
        "group": null
      },
      {
        "search_term": "kjellerup",
        "volume": 20,
        "group": null
      },
      {
        "search_term": "bags etc",
        "volume": 880,
        "group": "Direct Broad"
      }
    ]
  }
  ```
  - response
  ```json
  {
    "status": 0,
    "message": {
      "id": {
        "kind": "bigquery#tableDataInsertAllResponse"
      }
    }
  }
  ```

### Running on the server
> PM2 is a process manager for the JavaScript runtime Node.js
- List all processes: `pm2 list`
- Act on them: 
  ```sh
  pm2 start index.js
  pm2 stop
  pm2 restart
  pm2 delete
  ```

## Machine Learning for Parsing webpage
### Code
- https://github.com/dept-dk/spider

### Spider (for collecting data)
Crawling webpage content 
- Requirements
  - node 15.0+
- Dependencies
  - cheerio: 1.0.0-rc.9+
  - csv-writer: 1.6.0+
  - express: 4.17.1+
  - superagent: 6.1.0+
- Install Dependencies
  - run `npm install` or `yarn add`
- How to use
  - add urls in `index.js`
  - run `node index.js` in the terminal, webpages will be stored in a csv file
  - label the samples with 1 for useful, otherwise 0

### Training
- code: train.py
- steps:
  - read the dataset (csv file from the spider)
  - preprocess the data and train the neural network
  - store the trained model

### API
- url: [POST] /predict
- this function will read the trained model and make a prediction
- Run it on the server
  - `export FLASK_APP=api.py; nohup flask run --host=0.0.0.0 >> /var/log/flask.log 2>&1 &`
