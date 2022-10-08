# Set text output v1

A simple composite actions to apply cloudformation.

# Usage

**NOTE:** `jahanarun/aws-cloudformation-action@v1` is branch which always has latest `v1.x.x` version.  

```yml
- uses: jahanarun/aws-cloudformation-action@v1
      id: apply_cloudformation
      with:
        access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        default-region: ${{ secrets.AWS_DEFAULT_REGION }}
        template-files-source-path: ./infra
        template-files-destination-path: backend
        template-index-file: index.yaml
        staging-bucket-name: ${{ env.S3_BUCKET_NAME_FOR_CLOUDFORMATION_FILES }}
        stack-name: your-stack-name
        params: Environment=dev
          param1=${{ env.SOME_ENV_VAR }}
          param2=some-value
          param3=${{ secrets.SOME_SECRET }}
```
