# Interactive debugging of remotely executed tasks

Pipeline tasks executed with Wave and Fusionfs features enabled can be debugged interactively regardless of whether that are  executed on the local computer or remotely.

### Config 

This feature requires Wave and Fusionfs to both be enabled in your pipeline configuration.

```
workDir = 's3://some-bucket/work'

wave {
  enabled = true
}

fusion {
  enabled = true
}

docker {
  enabled = true
  envWhitelist = 'AWS_ACCESS_KEY_ID,AWS_SECRET_ACCESS_KEY'
}
```

Make sure youe replace the above `workDir` with an AWS S3 bucket you have access to. Note: the `docker` section is not needed when running with AWS Batch.

### Run it 

```
nextflow run rnaseq-nf 
```

For purposes of this example, the [rnaseq-nf](https://github.com/nextflow-io/rnaseq-nf) pipeline can be used. Run it locally or using the AWS Batch executor by adding the `-p batch` profile option.

Once execution completes, either successfully or with a failure, any task’s execution can be debugged in an interactive shell session using this command:

```
nextflow plugin nf-wave:debug-task <task hash or name or workdir>
```

In the above on-line command, use the unique task hash id generated during your pipeline’s execution. e.g. `d8/d067e8`. 

Note: currently, task debugging is only possible up until the expiry of the temporary token associated with the container’s execution — that is, 12 hours from the task's creation.
