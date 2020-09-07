# Set the project ID
gcloud config set project qwiklabs-gcp-02-4b7d98a92731

# Deploy a GKE cluster

# First list the versions for upgrades running
gcloud container get-server-config --zone us-central1-c
# Configure the cluster to the lowest version number from the list above
gcloud container clusters create cluster-1 --num-nodes 3 --zone us-central1-c --cluster-version="1.15.12-gke.2" --enable-ip-alias
# Get the details of the cluster
gcloud container clusters describe cluster-1 --zone us-central1-c
# List the version of the cluster
gcloud container get-server-config --zone us-central1-c

# Upgrade the cluster
gcloud container clusters upgrade cluster-1 --master --cluster-version=latest --zone us-central1-c 
# If prompted type 'Y' for yes.
# Describe the cluster to get the version
gcloud container clusters describe cluster-1
# The cluster has now been upgraded to the latest version

# List the node-pool in cluster 1
gcloud container node-pools list --cluster cluster-1 --zone  us-central1-c

# Describe the default node-pool
gcloud container node-pools describe default-pool --cluster cluster-1 --zone us-central1-c
# Upgrade the node-pool of the cluster 
gcloud container clusters upgrade cluster-1 --node-pool default-pool --zone us-central1-c
# After the upgrade describe the node-pool
gcloud container node-pools describe default-pool --cluster cluster-1 --zone us-central1-c

