
```

S3_BUCKET=s3://bucketname
S3_PATH="$S3_BUCKET"/sync-dir/

aws s3 mb "$S3_BUCKET"
mkdir tmp

# automated backup to s3://bucketname/sync-dir/backup everyday, expire after 30 days
# and, sync to s3://bucketname/sync-dir/master
docker run -d \
-e AWS_ACCESS_KEY_ID="$AWS_ACCESS_KEY_ID" \
-e AWS_SECRET_ACCESS_KEY="$AWS_SECRET_ACCESS_KEY" \
-e AWS_DEFAULT_REGION="$AWS_DEFAULT_REGION" \
-e S3_PATH="$S3_PATH" \
-e EXPIRE_DAY_AFTER=30 \
-e LOCAL_DIR=/usr/share/resources \
-v `pwd`/tmp:/usr/share/resources/master \
--name sync-dir keisato/sync-dir-s3

# pseudo file
touch tmp/t1

# backup manually
docker exec -it sync-dir backup

# pseudo file
touch tmp/t2

# restore 
docker exec -it sync-dir restore
```

