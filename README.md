# Hockey Resource

A resource for pushing (and pulling) versions of an app from [Hockey App](http://hockeyapp.net/).

## Source configuration

* `app_id`: *Required.* The alphanumeric identifier for your app. Found on your app's page in Hockey.
* `token`: *Required.* Your API token for Hockey. Can be found generated [here](https://rink.hockeyapp.net/manage/auth_tokens) (login required).

## Behaviour

### `out`: Upload an app binary to Hockey as a new version

Uploads a new version of the app specified in the resource's source configuration. Requires
a path to the binary to be uploaded.

#### Parameters

* `path`: *Required.* Path to the binary to upload.
* `downloadable`: *Optional.* If `true` the new version will be made available for download.
* `release_type`: *Optional.* Set the release type of the app; 2: alpha, 0: beta, 1: store, 3: enterprise
* `notes_file`: *Optional.* The file where your release notes are posted from
* `notes_type`: *Optional.* The format of your notes_file. Allowed options are Textile and Markdown. Markdown is default

### `in`: Download an app binary for a given version

*Under construction...*

### `check`: Check for new versions

*Under construction...*

## Pipeline example

```yaml
resource_types:
  - name: hockey
    type: docker-image
    source:
      repository: seadowg/hockey-resource
      tag: 0.1.0

resources:
  - name: app-repo
    type: git
    source:
      uri: https://github.com/user/app.git
      branch: master

  - name: hockey
    type: hockey
    source:
      app_id: APP_ID
      token: TOKEN

jobs:
  - name: package
    plan:
      - get: app-repo
        trigger: true
      - task: package
        file: app-repo/concourse/tasks/package.yml
      - put: hockey
        params:
          path: package/app.apk
```
