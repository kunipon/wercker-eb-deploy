# wercker-eb-deploy
```
deploy-demo:
  # the box already installed aws-cli
  box: cgswong/aws:latest
  steps:
    - script:
        name: "export version_label from light-tag"
        code: |
          export VERSION_LABEL=$(git describe --tags 2>/dev/null || echo $WERCKER_GIT_COMMIT)
          echo $VERSION_LABEL
          cp $WERCKER_SOURCE_DIR/target/demo.zip $WERCKER_SOURCE_DIR/
    - kunipon/eb-deploy@1.0.2:
        access-key: $S3_KEY_ID
        secret-key: $S3_KEY_SECRET
        app-name: <enter app name>
        env-name: <enter env name>
        version-label: $VERSION_LABEL
        # default ap-northeast-1
        region: <enter region>
        # ex) s3://elasticbeanstalk-ap-northeast-1-1234567890/path/to/key.zip
        # X_S3_BUCKET=elasticbeanstalk-ap-northeast-1-1234567890
        s3-bucket: $S3_BUCKET
        # X_S3_BUCKET_PATH=path/to
        s3-bucket-path: $S3_BUCKET_PATH
        # X_S3_KEY=key.zip
        s3-cp-src: $S3_KEY
        s3-cp-dst: $VERSION_LABEL-$S3_KEY
```
