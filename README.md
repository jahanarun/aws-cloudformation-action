# Apply AWS cloudformation v1

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

## Using output
The action is configured with output variable named `result`. It returns the output of the Cloudformation template.
Due to limitation of Github Actions, the ouput of a step cannot be of JSON type.
So, to use the output `result`, you will need to do a JSON conversion in your step.

```yml
- name: Apply Cloudformation and save output to make it available for next steps
  uses: jahanarun/aws-cloudformation-action@v1
  id: apply_cloudformation
  with:
    access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    default-region: ${{ secrets.AWS_DEFAULT_REGION }}
    template-files-source-path: ./infra
    template-files-destination-path: ui
    template-index-file: index.yaml
    staging-bucket-name: ${{ env.S3_BUCKET_NAME_FOR_CLOUDFORMATION_FILES }}
    stack-name: ui-stack
    params: Environment=dev
      Param1=${{ env.SOME_PARAM }}
      Param2=${{ secrets.SOME_SECRET }}

- name: Echo values from previous step
  run: |
      echo ${{ toJSON(fromJSON(steps.apply_cloudformation.outputs.result)) }}
      echo ${{ fromJSON(steps.apply_cloudformation.outputs.result).SomeKey }}
```
