# README

This action is credits to @willo32 and @wodndb

It is based form the fork from @wodndb (main code belongs to @willo32) but I ran into some type errors while customizing it. 
I converted it to js and added my customization.

## Overview

Github action to upload files to Google Drive using a service account.

- Main code from https://github.com/willo32/google-drive-upload-action
- Forked from https://github.com/wodndb/google-drive-upload-action

## Usage

#### Simple example:

```
steps:
    - uses: actions/checkout@v3

    - name: Upload files to Google Drive
      uses: dmcruz/js-google-drive-uploader-github-action@v1.4
      with:
        target: |-
          app/build/outputs/apk/!(demo)/release/*.apk
          app/build/outputs/apk/!(demo)/release/*.json

        credentials: ${{ secrets.<YOUR_SERVICE_ACCOUNT_CREDENTIALS> }}
        parent_folder_id: <YOUR_DRIVE_FOLDER_ID>
        #child_folder: optional
        #owner: optional
```

### Inputs

#### `target` (Required):

Local path to the files to upload, can be relative from github runner current directory.

Accepts multiple values. Pattern matching is accepted using glob.

Sample values:

```
app/build/outputs/apk/(!demo)/release/*.apk
src/*.js
src/data/*.json
```

#### `credentials` (Required):

A service account public/private key pair encoded in base64.

[Generate and download your credentials in JSON format](https://cloud.google.com/iam/docs/creating-managing-service-account-keys#creating_service_account_keys)

Run `base64 my_service_account_key.json > encoded.txt` and paste the encoded string into a github secret.

#### `parent_folder_id` (Required):

The id of the drive folder where you want to upload your files. It is the string of characters after the last `/` when browsing to your folder URL. You must share the folder with the service account (using its email address) unless you specify a `owner`.

#### `child_folder` (Optional):

A sub-folder where to upload your files. It will be created if non-existent and must remain unique. Useful to organize your drive like so:

```
📂 Release // parent folder
 ┃
 ┣ 📂 v1.0 // child folder
 ┃ ┗ 📜 uploaded_file_v1.0
 ┃
 ┣ 📂 v2.0 // child folder
 ┃ ┗ 📜 uploaded_file_v2.0
```

#### `owner` (Optional):

The email address of a user account that has access to the drive folder and will get the ownership of the file after its creation. To use this feature you must grant your service account a [domain-wide delegation of authority](https://developers.google.com/admin-sdk/directory/v1/guides/delegation) beforehand.

## For developer

- You should run `yarn build` and push build file before release.
