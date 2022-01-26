# GitHub Action for downloading a file from S3 or DigitalOcean Spaces

### Required Environment Variables

#### Below should be secrets

- `AWS_ACCESS_KEY_ID` S3 Access key
- `AWS_SECRET_ACCESS_KEY` S3 Secret Access key
- `AWS_ENDPOINT` S3 Region + Domain (eg. s3.us-east-1.amazonaws.com)
- `AWS_BUCKET` Bucket name

#### These should be defined in your Action workflow

- `LOCAL_FILE` location of destination file
- `REMOTE_FILE` file to download

#### Optional

- `INCLUDE_SHA256` will include your file postfixed with `.sha256` and the sha256 hash for that binary

### Example

```yaml
- name: Release Binary to S3
  uses: mostlytyped/s3-put-action@v1.0.0
  with:
    args: --acl-public
  env:
    LOCAL_FILE: dist/MyApp
    REMOTE_FILE: downloads/latest/MyApp
    AWS_ENDPOINT: ${{ secrets.AWS_ENDPOINT }}
    AWS_BUCKET: ${{ secrets.AWS_BUCKET }}
    AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
    AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    INCLUDE_SHA256: true
```

### Advanced

This action is ultra simple and uses [s3cmd](https://github.com/s3tools/s3cmd) under the hood. The execution of a call looks like:

```sh
s3cmd get s3://${AWS_BUCKET}/${REMOTE_FILE} ${LOCAL_FILE}
```

adding arguments to (as in the example above) can be done easily with "with" params.
