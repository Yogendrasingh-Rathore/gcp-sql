# Commands to execute in gcloud shell :

# Mysql Instance :

curl -X POST \
-H "Authorization: Bearer "$(gcloud auth print-access-token) \
-H "Content-Type: application/json; charset=utf-8" \
-d @sql-instance.json \
https://www.googleapis.com/sql/v1beta4/projects/[PROJECT-ID]/instances

# PASSWORD :

gcloud auth login
ACCESS_TOKEN="$(gcloud auth print-access-token)"
curl --header "Authorization: Bearer "$(gcloud auth print-access-token) \
     --header 'Content-Type: application/json' \
     --data '{"name": "root", "password": "[PASSWORD]"}' \
     -X PUT \
     'https://www.googleapis.com/sql/v1beta4/projects/[PROJECT-ID]/instances/cloudsql-demo3/users?name=root&host=%25'

# storage bucket :

curl -X POST --data-binary @storage.json \
     -H "Authorization: Bearer "$(gcloud auth print-access-token) \
     -H "Content-Type: application/json" \
     "https://storage.googleapis.com/storage/v1/b?project=[PROJECT-ID]"


# Before import - export, you need to give permission to the service account to import - export on that bucket
# Add the service account to the bucket ACL as a writer
# gsutil acl ch -u p748482066883-q4sn1y@gcp-sa-cloud-sql.iam.gserviceaccount.com:W gs://sqldump-storage2



# export :

curl -X POST \
-H "Authorization: Bearer "$(gcloud auth print-access-token) \
-H "Content-Type: application/json; charset=utf-8" \
-d @export.json \
https://www.googleapis.com/sql/v1beta4/projects/[PROJECT-ID]/instances/cloudsql-demo4/export

# import :

curl -X POST \
-H "Authorization: Bearer "$(gcloud auth print-access-token) \
-H "Content-Type: application/json; charset=utf-8" \
-d @import.json \
https://www.googleapis.com/sql/v1beta4/projects/[PROJECT-ID]/instances/cloudsql-demo4/import


# To delete the instance :

curl -X DELETE \
-H "Authorization: Bearer "$(gcloud auth print-access-token) \
https://www.googleapis.com/sql/v1beta4/projects/[PROJECT-ID]/instances/cloudsql-demo4

# To delete objects from the storage bucket :

curl -X DELETE \
-H "Authorization: Bearer [OAUTH2_TOKEN]" \
"https://storage.googleapis.com/storage/v1/b/[BUCKET_NAME]/o/[OBJECT_NAME]"

# To delete the storage bucket :

curl -X DELETE -H "Authorization: Bearer "$(gcloud auth print-access-token) \
  "https://storage.googleapis.com/storage/v1/b/[BUCKET_NAME]"
