# Submission Parser (v2)

NOTE: This is the Google Cloud Run (with job) and Google Cloud Scheduler version. For the non-cloud version please see `legacy` branch.

## Other requirements

- The Google Sheet for the submissions should be public, so the job have access.
- Target folder should be shared with the service account.
- Service account should have permission to read/write Datastore's Firestore and cloud run invoker role.

## Build the container image

```console
gcloud builds submit --pack image=gcr.io/cotsconf/submission-parser-job
```

## Create the job

```console
gcloud beta run jobs create submission-parser \
    --image gcr.io/cotsconf/submission-parser-job \
    --set-env-vars APP_FORM_RESPONSE_ID= \
    --set-env-vars APP_SHEET_NAME="Form Response 1" \
    --set-env-vars WORKING_COLLECTION_NAME=collection-name \
    --set-env-vars WORKING_DOCUMENT_NAME=document-name \
    --set-env-vars PROPOSAL_FOLDER_ID=output-folder-1-drive-id \
    --set-env-vars INDIVIDUAL_PROPOSAL_FOLDER_ID=output-folder-2-drive-id \
    --set-env-vars PANEL_PROPOSAL_FOLDER_ID=output-folder-3-drive-id \
    --set-env-vars ROUNDTABLE_PROPOSAL_FOLDER_ID=output-folder-4-drive-id \
    --max-retries 0 \
    --region europe-west9 \
    --service-account email-address-of-service-account
```

# Crete schedule for the job.

```console
# Crete schedule for the job.
gcloud scheduler jobs create http submission-parser-scheduler \
       --location mars-west36 \
       --schedule="30 10 * * *" \
       --uri="https://europe-west9-run.googleapis.com/apis/run.googleapis.com/v1/namespaces/PROJECT-ID/jobs/JOB-NAME:run" \
       --http-method POST \
       --oauth-service-account-email email-address-of-service-account
```
