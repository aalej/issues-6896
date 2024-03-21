# Repro for 6896

## Steps to reproduce

1. Create a Service account with the ff permissions:
   - Cloud Functions Admin
   - Editor
   - Service Usage Consumer
1. Download a serivce account key JSON file.
1. Switch to an old version of `firebase-tools`
   - I'm using 13.4.0 for this since it's the version prior https://github.com/firebase/firebase-tools/issues/6801
1. Run `export GOOGLE_APPLICATION_CREDENTIALS=<path_to_json_key>`
1. Run `firebase deploy --project project_id`
   - Deploys without any issues
1. Run `npm i -g firebase-tools@latest` to install latest version
1. Run `export GOOGLE_CLOUD_QUOTA_PROJECT=<project_id>`
1. Make a few changes to `index.js` to avoid `skipdeployingnoopfunctions`
1. Run `firebase deploy --project project_id`

```shell
⚠  functions: Upload Error: HTTP Error: 403, <?xml version='1.0' encoding='UTF-8'?><Error><Code>SignatureDoesNotMatch</Code><Message>Access denied.</Message><Details>The request signature we calculated does not match the signature you provided. Check your Google secret key and signing method.</Details><StringToSign>PUT

application/zip
1711039215
x-goog-user-project:<PROJECT_ID>
/gcf-v2-uploads-<PROJECT_NUMBER>-us-central1/5b44ae12-747f-4de7-aede-038ed59543cb.zip</StringToSign></Error>

Error: HTTP Error: 403, <?xml version='1.0' encoding='UTF-8'?><Error><Code>SignatureDoesNotMatch</Code><Message>Access denied.</Message><Details>The request signature we calculated does not match the signature you provided. Check your Google secret key and signing method.</Details><StringToSign>PUT

application/zip
1711039215
x-goog-user-project:<PROJECT_ID>
/gcf-v2-uploads-<PROJECT_NUMBER>-us-central1/5b44ae12-747f-4de7-aede-038ed59543cb.zip</StringToSign></Error>
```

## Notes

Unsetting `GOOGLE_CLOUD_QUOTA_PROJECT` seems to workaround the issue

- Run `unset GOOGLE_CLOUD_QUOTA_PROJECT`
- Run `firebase deploy`
