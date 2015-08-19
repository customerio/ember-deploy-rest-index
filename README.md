# ember-deploy-rest-index

This [ember-cli-deploy](https://github.com/ember-cli/ember-cli-deploy) index adapter uses HTTP REST commands to handle all operations pertaining to the index.html deploy process. The server is, therefore, responsible for managing index.html revisions.

## Purpose

This adapter is intended for those who have an existing REST API and would like to leverage it to manage their index.html revisions. It is ideal for internal system environments (not in the cloud) or in the case that adding additional components such as a NoSQL is prohibitive. For such environments, this adapter will enable the usage of the existing infrastructure to accommodate, still, a **Lightning Fast Deployments** process.

## To use
`npm install ember-deploy-rest-index`

## Commands

The adapter implements the following commands as per the `ember-cli-deploy` interface:

* `deploy` - Makes a POST to upload the index.html information:  

  **Sample Request:**

  ```js
  HTTP POST http://leojh.com/api/ember-revisions
  {
    "id": 'ember-app:9c0568d',
    "body": '<html>...</html>'
  }
  ```

  **Sample Response:** HTTP 200 OK, 201 CREATED, or 204 NO CONTENT with no content.

* `deploy:list` - Makes a GET call to get all uploaded revisions

  **Sample Request:** ``HTTP GET https://leojh.com/api/ember-revisions``

  **Sample Response:**
  ```js
  HTTP OK
  [
    {"id":"ember-app:1bdc67a","created_at":"2015-06-19T09:58:00-04:00"},
    {"id":"ember-app:2251eda","created_at":"2015-06-22T10:51:00-04:00","current":true}
    ...
  ]
  ```

* `deploy:activate` - Makes a PUT call to activate the specified revisions

  **Sample Request:** `HTTP PUT https://leojh.com/api/ember-revisions/ember-app:1bdc67a`

  **Sample Response:** HTTP 200 OK, or 204 NO CONTENT with no content.

## Config

Sample config in your deploy.js file:
```js
store: {
  type: 'REST',
  host: 'https://leojh.com',
  resource: '/api/v1/ember-revisions',
  user: 'deploy',
  password: 'something-very-secure'
}
```
* `type` - Must be 'REST' to use this adapter
* `host` - the root URL for the service
* `resource` - the path to the resource responsible for handling your revisions
* `user` - user name for HTTP Basic Auth (optional)
* `password` - password for HTTP Basic Auth (optional, only used if `user` is set)

The combination of config settings will help construct the target URLs for calling your service. For example, given the config above, the `deploy:list` command will result in: `GET https://deploy:something-very-secure@leojh.com/api/v1/ember-revisions`.

Note: don't use Basic Auth with http. In fact, don't use http, just use https.

## TODOs:

1. Authentication header
2. Cleanup of old revisions
2. Tests (I know, they should come first)
3. Multipart?
4. A REST adapter for assets?
