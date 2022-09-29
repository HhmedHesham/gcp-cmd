echo "Creating quiz-account Service Account"
gcloud iam service-accounts create quiz-account --display-name "Quiz Account"
gcloud iam service-accounts keys create key.json --iam-account=quiz-account@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com

echo "Setting quiz-account IAM Role"
gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member serviceAccount:quiz-account@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role roles/owner

echo "Creating Datastore/App Engine instance"
gcloud app create --region "us-central"

echo "Creating bucket: gs://$DEVSHELL_PROJECT_ID-media"
gsutil mb gs://$DEVSHELL_PROJECT_ID-media

echo "Exporting GCLOUD_PROJECT and GCLOUD_BUCKET"
export GCLOUD_PROJECT=$DEVSHELL_PROJECT_ID
export GCLOUD_BUCKET=$DEVSHELL_PROJECT_ID-media

echo "Creating virtual environment"
mkdir ~/venvs
virtualenv -p python3 ~/venvs/developingapps
source ~/venvs/developingapps/bin/activate

echo "Installing Python libraries"
pip install --upgrade pip
pip install -r requirements.txt

echo "Creating Datastore entities"
python add_entities.py

echo "Export credentials key.json"
export GOOGLE_APPLICATION_CREDENTIALS=key.json

echo "Creating Cloud Pub/Sub topic"
gcloud pubsub topics create feedback
gcloud pubsub subscriptions create worker-subscription --topic feedback

echo "Creating Cloud Spanner Instance, Database, and Table"
gcloud spanner instances create quiz-instance --config=regional-us-central1 --description="Quiz instance" --nodes=1
gcloud spanner databases create quiz-database --instance quiz-instance --ddl "CREATE TABLE Feedback ( feedbackId STRING(100) NOT NULL, email STRING(100), quiz STRING(20), feedback STRING(MAX), rating INT64, score FLOAT64, timestamp INT64 ) PRIMARY KEY (feedbackId);"

echo "Project ID: $DEVSHELL_PROJECT_ID"

/*********
This script file:

Creates a Google App Engine application.
Exports environment variables GCLOUD_PROJECT and GCLOUD_BUCKET.
Updates pip then runs pip install -r requirements.txt.
Creates entities in Google Cloud Datastore.
Creates a Google Cloud Pub/Sub topic.
Creates a Cloud Spanner Instance, Database, and Table.
Prints out the Project ID.
*********/

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

