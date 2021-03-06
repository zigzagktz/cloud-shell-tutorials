# Dataflow Word Count Tutorial

<walkthrough-tutorial-url url="https://cloud.google.com/dataflow/docs/quickstarts/quickstart-java-maven"></walkthrough-tutorial-url>
<walkthrough-watcher-constant key="directory" value="dataflow-intro"></walkthrough-watcher-constant>
<walkthrough-watcher-constant key="job-name" value="dataflow-intro"></walkthrough-watcher-constant>

## Introduction

In this tutorial, you'll learn the basics of the Cloud Dataflow service by
running a simple example pipeline using Java.

Dataflow pipelines are either *batch* (processing bounded input like a file or
database table) or *streaming* (processing unbounded input from a source like
Cloud Pub/Sub). The example in this tutorial is a batch pipeline that counts
words in a collection of Shakespeare's works.

Before you start, you'll need to check for prerequisites in your Cloud Platform
project and perform initial setup.

## Project setup

Google Cloud Platform organizes resources into projects. This allows you to
collect all the related resources for a single application in one place.

<walkthrough-project-billing-setup></walkthrough-project-billing-setup>
<walkthrough-project-permissions permissions="dataflow.jobs.create"></walkthrough-project-permissions>

## Set up Cloud Dataflow

To use Dataflow, turn on the Cloud Dataflow APIs and open the Cloud Shell.

### Turn on Google Cloud APIs

<walkthrough-enable-apis apis=
  "compute.googleapis.com,dataflow,cloudresourcemanager.googleapis.com,logging,storage_component,storage_api,bigquery,pubsub">
</walkthrough-enable-apis>

### Open the Cloud Shell

Cloud Shell is a built-in command line tool for the console. You're going to use
Cloud Shell to deploy your app.

Open Cloud Shell by clicking
<walkthrough-cloud-shell-icon></walkthrough-cloud-shell-icon>
[icon][spotlight-open-devshell] in the navigation bar at the top of the console.

## Install Cloud Dataflow samples on Cloud Shell

To use the Cloud Dataflow SDK for Java, your development environment will
require Java, the Google Cloud SDK, the Cloud Dataflow SDK for Java, and Apache
Maven for managing SDK dependencies. This tutorial uses a Cloud Shell that has
Java, the Google Cloud SDK, and Maven already installed.

Alternatively, you can do this tutorial [on your local
machine.][dataflow-java-tutorial]

### Download the samples and the Cloud Dataflow SDK for Java using the Maven command

When you run this command, Maven will create a project structure and config file
for downloading the appropriate version of the Cloud Dataflow SDK.

```bash
mvn archetype:generate \
    -DarchetypeArtifactId=google-cloud-dataflow-java-archetypes-examples \
    -DarchetypeGroupId=com.google.cloud.dataflow \
    -DarchetypeVersion=2.1.0 \
    -DgroupId=com.example \
    -DartifactId={{directory}} \
    -Dversion=&quot;0.1&quot; \
    -DinteractiveMode=false \
    -Dpackage=com.example
```

  *  `archetypeArtifactId` and `archetypeGroupId` are used to define the example
    project structure.
  *  `groupId` is your organization's Java package name prefix; for example,
    `com.mycompany`
  *  `artifactId` sets the name of the created jar file. Use the default value
    (`{{directory}}`) for the purpose of this tutorial.

Run the Maven command in Cloud Shell.

### Change directory

Change your working directory to `{{directory}}`.

```bash
cd {{directory}}
```

If you'd like to see the code for this example, you can find it in the `src`
subdirectory in the `{{directory}}` directory.

## Set up a Cloud Storage bucket

Cloud Dataflow uses Cloud Storage buckets to store output data and cache your
pipeline code.

### Run gsutil mb

In Cloud Shell, use the command `gsutil mb` to create a Cloud Storage bucket.

```bash
gsutil mb gs://{{project-id-no-domain}}
```

For more information about the `gsutil` tool, see the
[documentation][gsutil-docs].

## Create and launch a pipeline

In Cloud Dataflow, data processing work is represented by a *pipeline*. A
pipeline reads input data, performs transformations on that data, and then
produces output data. A pipeline's transformations might include filtering,
grouping, comparing, or joining data.

If you'd like to see the code for this example, you can find it in the `src`
subdirectory in the `{{directory}}` directory.

### Launch your pipeline on the Dataflow Service

Use Apache Maven's `mvn exec` command to launch your pipeline on the service.
The running pipeline is referred to as a *job.*

```bash
mvn compile exec:java \
  -Dexec.mainClass=com.example.WordCount \
  -Dexec.args=&quot;--project={{project-id}} \
  --stagingLocation=gs://{{project-id-no-domain}}/staging/ \
  --output=gs://{{project-id-no-domain}}/output \
  --runner=DataflowRunner \
  --jobName={{job-name}}&quot;
```

  *  `stagingLocation` is the storage bucket Cloud Dataflow will use for the
    binaries and other data for running your pipeline. This location can be
    shared across multiple jobs.
  *  `output` is the bucket used by the WordCount example to store the job
    results.

### Your job is running

Congratulations! Your binary is now staged to the storage bucket that you
created earlier, and Compute Engine instances are being created. Cloud Dataflow
will split up your input file such that your data can be processed by multiple
machines in parallel.

Note: When you see the "Job finished" message, you can close Cloud Shell.

If you wish to clean up the Maven project you generated, run `cd .. && rm -R
{{directory}}` in the Cloud Shell to delete the directory.

## Monitor your job

Check the progress of your pipeline on the Cloud Dataflow Monitoring UI page.

### Go to the Cloud Dataflow page

Open the [menu][spotlight-console-menu] on the left side of the console.

Then, select the **Dataflow** section.

<walkthrough-menu-navigation sectionId="DATAFLOW_SECTION"></walkthrough-menu-navigation>

### Select your job

Click your job to view its details.

### Explore pipeline details and metrics

Explore the pipeline on the left and the job information on the right. To see
detailed job status, click [Logs][spotlight-job-logs]. Try clicking a step in
the pipeline to view its metrics.

As your job finishes, you'll see the job status change, and the Compute Engine
instances used by the job will stop automatically.

## View your output

Now that your job has run, you can explore the output files in Cloud Storage.

### Go to the Cloud Storage page

Open the [menu][spotlight-console-menu] on the left side of the console.

Then, select the **Storage** section.

<walkthrough-menu-navigation sectionId=STORAGE_SECTION></walkthrough-menu-navigation>

### Go to the storage bucket

In the list of buckets, select the bucket you created earlier. If you used the
suggested name, it will be named `{{project-id}}`.

The bucket contains a staging folder and output folders. Dataflow saves the
output in shards, so your bucket will contain several output files.

## Clean up

In order to prevent being charged for Cloud Storage usage, delete the bucket you
created.

### Go back to the buckets browser

Click the [Buckets][spotlight-buckets-link] link.

### Select the bucket

Check the box next to the bucket you created.

### Delete the bucket

Click [Delete][spotlight-delete-bucket] and confirm your deletion.

## Conclusion

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

Here's what you can do next:

  *  [Read more about the Word Count example][wordcount]
  *  [Learn about the Cloud Dataflow programming model][df-model]
  *  [Explore the Cloud Dataflow SDK on GitHub][df-sdk]

Set up your local environment:

  *  [Use Eclipse to run Dataflow][df-eclipse]
  *  [Use Python to run Dataflow][df-python]

[dataflow-java-tutorial]: https://cloud.google.com/dataflow/docs/quickstarts/quickstart-java-maven
[df-eclipse]: https://cloud.google.com/dataflow/docs/quickstarts/quickstart-java-eclipse
[df-model]: https://cloud.google.com/dataflow/model/programming-model-beam
[df-python]: https://cloud.google.com/dataflow/docs/quickstarts/quickstart-python
[df-sdk]: https://github.com/apache/beam/tree/master/sdks/java
[gsutil-docs]: https://cloud.google.com/storage/docs/gsutil
[spotlight-buckets-link]: walkthrough://spotlight-pointer?cssSelector=.p6n-cloudstorage-path-link
[spotlight-console-menu]: walkthrough://spotlight-pointer?spotlightId=console-nav-menu
[spotlight-delete-bucket]: walkthrough://spotlight-pointer?cssSelector=#p6n-cloudstorage-delete-buckets
[spotlight-job-logs]: walkthrough://spotlight-pointer?cssSelector=#p6n-dax-job-logs-toggle
[spotlight-open-devshell]: walkthrough://spotlight-pointer?spotlightId=devshell-activate-button
[wordcount]: https://beam.apache.org/get-started/wordcount-example/
