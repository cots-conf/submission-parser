# COTS

Support applications for the meeting of Council on Thai Studies (COTS)

## Proposal submission parser

Parse the submitted proposals via Google Form into Google Doc. The parser is designed
to be triggered by an external timer. Currently, it only supports using firebase as a
working memory storage.

### Setup the Google Form

1. Create the form and response Google Sheet.
2. Take note of the Google Sheet's ID and sheet name.

### Setup the folders in Google Drive

1. Create a project in Google Cloud Console with Drive API enabled.
2. Create a service account with key.
3. Create folders in Google Drive and share them with the service account. Take
   note of the folders' ID.
4. Make `.env` file by copy `env.sample` and fill in the noted values.

If you are planning to use Firebase's Firestore to store the working data, create
the project and enable the Firestore. Create a collection with one document
that has key `last_processed_row` that map to a number.