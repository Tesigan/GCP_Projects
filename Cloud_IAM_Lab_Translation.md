# Navigate to the IAM and explore roles and service accounts.
gcloud iam roles list
gcloud iam service-accounts list
# Make sure that you are signed in as username1
gcloud set account Username1


# Prepare a resource for access testing
# Create a bucket and upload a sample file
gsutil mb gs://    --region multi-regional
# Upload a file to your bucket
gsutil cp /home/tesigan/Documents/test.txt gs://

#  Remove project access
# Remove project viewer role for username 
gcloud projects remove-iam-policy-binding PROJECT_ID \
--member=user:Username2 --role=roles/ProjectViewer
# Verify that Username 2 has lost access
gcloud set account Username2 
# Navigate to cloud bucket
gsutil ls gs://BUCKET_NAME

# Add storage access
# Switch to Username1
gcloud set account Username1
# Add storage permissions for Username2 
gcloud projects add-iam-policy-binding PROJECT_ID \
--member=user:Username2 --role=roles/StorageObjectViewer
# Switch to Username2
gcloud set account Username2
# Verify that Username 2 has storage access
gsutil ls gs://BUCKET_NAME


# Set up the Service Account User
gcloud set account Username1
# Create a service account
gcloud iam service-accounts create read-bucket-objects
# Add storage permissions for read-bucket-objects
gcloud projects add-iam-policy-binding PROJECT_ID \
--member=user:read-bucket-objects --role=roles/StorageObjectViewer
# Add the user to the service account
gcloud iam service-accounts add-iam-policy-binding read-bucket-objects \
--member=altostrat.com --role=roles/service-account-user
# Grant Compute Engine access
gcloud iam service-accounts add-iam-policy-binding read-bucket-objects \
--member=altostrat.com --role=roles/compute-instance-admin-v1
# Create a VM with the Service Account User
gcloud compute intances create demoiam \
--region us-central1 --zone us-central1-a \
--machine-type f1-micro \
--service account read-bucket-objects

# Explore the Service Account User role
# Use the Service Account User
ssh demoiam
# See if you have access to view instances using the service account
gcloud compute instances list
# Copy a file to the storage bucket 
gsutil cp gs://BUCKET_NAME/sample.txt .
# Rename the file that was just copied
mv sample.txt sample2.txt
# Copy the name back to the bucket
gsutil cp sample2.txt gs://BUCKET_NAME




