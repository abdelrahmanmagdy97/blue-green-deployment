# blue-green-deployment

# Blue-Deployment:
# Step 1: create s3 manually with index.html, enable static hosting
# step 2: create clouformation stack called cloudfront.yml ,A file which have iside of it parameters "PipelineID" 
# that needs to be passed in a script, The script is below
# Step 3: Script:
# aws cloudformation deploy  \
# --template-file cloudfront.yml  \
# --stack-name production-distro  \
# --parameter-overrides PipelineID="1100691979"
#
# So PipelineID is passed from script to the cloudfront.yml that creates stack - Stack name will be as passed.
# And now your blue deployment is up and running :)
#
# Green-Deployment
# Step 1: Here s3 is created automatically using cloudformation file bucket.yml which have new index.html , It  #creates stack and has parameters inside it MyBucketName
# step 2: This time the script used in blue deployment will be used again but inside a job in circle CI with #aws-cli image.
# Step 3: Script which is inside config.yml:
#  aws cloudformation deploy \
#            --template-file bucket.yml \
#            --stack-name stack-create-bucket-${CIRCLE_WORKFLOW_ID:0:7} \
#            --parameter-overrides MyBucketName="mybucket-${CIRCLE_WORKFLOW_ID:0:7}"
#
# So MyBucketName is passed from script to the bucket.yml that creates stack - Stack name will be as passed
# And now your Green deployment is up and running :)


# Now blue and green deployments are up and running, we want green to be on production ,so we will overwrite pipeline ID on production-distro stack in a job in circle CI
#            aws cloudformation deploy \
#            --template-file cloudfront.yml \
#            --stack-name production-distro \
#            --parameter-overrides PipelineID="${CIRCLE_WORKFLOW_ID:0:5}" 

# Finally clean up blue deployment after saving its name in a text file and sharing it between 2 jobs

