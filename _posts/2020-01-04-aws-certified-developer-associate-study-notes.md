---
layout: post
title: "AWS Certified Developer - Associate Exam Study Notes"
date: 2020-01-15
excerpt: "Notes for AWS Certified Developer - Associate Exam"
tag:
    - aws certified developer associate 2020
    - aws developer associate certification
    - aws certified developer associate guide
    - aws certified developer associate guide 2020
    - aws developer associate notes
    - aws developer training
    - aws certification
    - aws certification for java developers
    - aws certified developer associate study notes
    - aws certified developer associate exam notes
    - aws certified developer associate exam guide
    - aws certified developer associate study guide
    - aws certified developer associate study material
comments: true
---

## EC2

### SSH: Unprotected Private Key File

Your private key file must be protected from read and write operations from any other users. If your private key can be read or written to by anyone but you, then SSH ignores your key and returns error message "UNPROTECTED PRIVATE KEY FILE!".

To fix the error, execute the following command, substituting the path for your private key file.

```bash
[ec2-user ~]$ chmod 0400 .ssh/my_private_key.pem
```

### Instance Connect

-   To connect using the Amazon EC2 console, the instance must have a `public IP address`.

-   If the instance has only `private IP address`, then you have to connect using Instance Connect CLI.

-   Instance Connect CLI generates a one-time-use SSH public key, pushes the key to the instance where it remains for 60 seconds, and connects the user to the instance.

-   Linux distributions supported - Amazon Linux 2 (or) Ubuntu 16.04 or later.

### Instances

-   _Burstable Performance Instances_ -

    Are designed to provide a baseline level of CPU performance with the ability to burst to a higher level when required by your workload.

    Examples include microservices, low-latency interactive applications, small and medium databases.

    Burstable performance instances are the only instance types that use credits for CPU usage.

-   _Unlimited Mode for Burstable Performance Instances_ -

    A burstable performance instance configured as unlimited can sustain high CPU performance for any period of time whenever required.

    If the instance runs at higher CPU utilization for a prolonged period, it can do so for a flat additional rate per vCPU-hour.

### Instance Metadata

-   Instance metadata is data about your instance that you can use to configure or manage the running instance.

-   You can access the local IP address of your instance from instance metadata to manage a connection to an external application.

-   It is available at URI - `http://169.254.169.254/latest/meta-data/`

-   The IP address 169.254.169.254 is a link-local address and is valid only from the instance.

### AMI

-   AMI are `restricted to a region`. i.e. A AMI in eu-west-1 is not available in eu-central-1.

### Security Groups

-   Security groups are `restricted to a region`. i.e. A security group in eu-west-1 is not available in eu-central-1.

### EBS

-   EBS volumes are highly available and reliable storage volumes that can be attached to any running instance that is in the `same Availability Zone`.

Process to modify size, performance, and volume type of your Amazon EBS volumes without detaching them:

1. Create a snapshot of the volume in case you need to roll back your changes.

2. Request the volume modification via console or cli.

3. Monitor the progress.

4. If the size of the volume was modified, extend the volume's file system.

Process to encrypt an existing unencrypted volume:

1. Create a snapshot of the volume.

2. Copy the snapshot by setting the `Encrypted` parameter and, optionally, the `KmsKeyId` parameter.

3. Resulting snapshot is encrypted.

4. Create a new volume from the encrypted snapshot. Resulting volume is encrypted.

### Billing

-   When you stop an instance, we shut it down. We don't charge usage for a stopped instance, or data transfer fees.

{% include donate.html %}
{% include advertisement.html %}

## S3

<!-- prettier-ignore-start -->

|                                                    |             |
| -------------------------------------------------- | ----------- |
| Maximum object size                                | 5 TB        |
| Maximum object size supported by PUT operation     | 5 GB        |
| Multipart upload mandatory for objects of size     | 5 GB - 5 TB |
| Size above which multipart uploads are recommended | 100 MB      |
{:.table-striped}

<!-- prettier-ignore-end -->

Advantages of multipart upload:

-   Improved throughput
-   Quick recovery from any network issues
-   Pause and resume object uploads
-   Begin an upload before you know the final object size

### Consistency Model

-   Amazon S3 provides `read-after-write consistency for PUTS` of new objects in your S3 bucket in all Regions with one caveat.

-   The caveat is that if you make a HEAD or GET request to the key name (to find if the object exists) before creating the object, Amazon S3 provides `eventual consistency for read-after-write`.

-   Amazon S3 offers `eventual consistency for overwrite PUTS and DELETES` in all Regions.

### Encryption

-   You can protect data in transit using Secure Sockets Layer (SSL) or client-side encryption.

-   You can protect data at rest using server-side encryption.

### Server-Side Encryption

<!-- prettier-ignore-start -->

|         |                                                                                                                                                                                                                                                                                                                                                                       |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SSE-S3  | Protecting Data Using Server-Side Encryption with Amazon S3-Managed Encryption Keys<br/>Set x-amz-server-side-encryption header to AES256 in your request if you want Amazon S3 to encrypt your data at rest                                                                                                                                                          |
| SSE-KMS | Server-Side Encryption with Customer Master Keys (CMKs) Stored in AWS Key Management Service<br/>Set x-amz-server-side-encryption header to aws:kms in your request if you want Amazon S3 to encrypt your data with AWS Key Management Service (SSE-KMS) customer master keys (CMKs) at rest                                                                          |
| SSE-C   | Server-Side Encryption with Customer-Provided Keys<br/> You must provide encryption key information using the following request headers: x-amz-server-side​-encryption​-customer-algorithm (AES256), x-amz-server-side​-encryption​-customer-key, x-amz-server-side​-encryption​-customer-key-MD5<br/>Amazon S3 rejects any requests made over HTTP when using SSE-C |
{:.table-striped}

<!-- prettier-ignore-end -->

### Performance

-   Amazon S3 now provides increased performance to support at least 3,500 requests per second to add data and 5,500 requests per second to retrieve data.

-   This S3 request rate performance increase removes any previous guidance to randomize object prefixes to achieve faster performance. That means you can now use logical or sequential naming patterns in S3 object naming without any performance implications.

## Lambda

<!-- prettier-ignore-start -->

|                            |                                                                           |
| -------------------------- | ------------------------------------------------------------------------- |
| /tmp directory storage     | 512 MB                                                                    |
| Concurrent executions      | 1,000                                                                     |
| Function timeout           | 15 minutes                                                                |
| Deployment package size    | 50 MB (zipped, for direct upload)<br/>250 MB (unzipped, including layers) |
| Environment variables size | 4 KB                                                                      |
{:.table-striped}

<!-- prettier-ignore-end -->

{% include donate.html %}
{% include advertisement.html %}

## DynamoDB

-   DynamoDB provides `Optimistic Locking with Version Number` to ensure that the client-side item that you are updating (or deleting) is the same as the item in Amazon DynamoDB.

-   DynamoDB provides `Conditional Writes` for PutItem, UpdateItem, DeleteItem to ensure that the write succeeds only if the item attributes meet one or more expected conditions. Conditional writes are helpful in cases where multiple users attempt to modify the same item.

<!-- prettier-ignore-start -->

|                |                                                                                                                                                     |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Scan operation | Limited to 1 MB per call                                                                                                                            |
| BatchWriteItem | A single BatchWriteItem operation can contain up to 25 PutItem or DeleteItem requests. The total size of all the items written cannot exceed 16 MB. |
| TTL | DynamoDB typically deletes expired items within 48 hours of expiration. |
{:.table-striped}

<!-- prettier-ignore-end -->

### Calculate WCU

```
# of items per second (Round up to the nearest 1 KB multiplier) * ITEM SIZE (rounded up to the next 1KB multiplier)
```

### Calculate RCU

Strongly consistent reads:

```
# of items per second (Round up to the nearest 4 KB multiplier) * (ITEM SIZE (rounded up to the next 4KB multiplier) / 4 KB)
```

Eventually consistent reads:

```
(# of items per second (Round up to the nearest 4 KB multiplier) / 2) * (ITEM SIZE (rounded up to the next 4KB multiplier) / 4 KB)
```

### Secondary Indexes

<!-- prettier-ignore-start -->

| Global Secondary Index                                          | Local Secondary Index                                              |
| --------------------------------------------------------------- | ------------------------------------------------------------------ |
| Partition and sort key can be different from base table         | Same partition key as base table                                   |
| Query entire table across all partitions                        | Query over single partition specified by partition key             |
| Queries are eventually consistent                               | Queries are eventual or strongly consistent                        |
| Queries/Scan consume RCU/WCU from index and not from base table | Queries/Scan consume RCU from base table                           |
| Can only request attributes which are projected into the index  | Can request attributes that are not projected into the index       |
| GSI can be added to existing table or be deleted from table     | Cannot add LSI to existing table or delete existing LSI from table |
{:.table-striped}

<!-- prettier-ignore-end -->

### Streams

-   A DynamoDB stream is an ordered flow of information about changes to items in a DynamoDB table.

-   When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table.

-   DynamoDB Streams captures a time-ordered sequence of item-level modifications in any DynamoDB table and stores this information in a log for up to `24 hours`.

### Transactions

-   Amazon DynamoDB transactions provide atomicity, consistency, isolation, and durability (ACID) in DynamoDB, helping you to maintain data correctness in your applications.

-   DynamoDB performs two underlying reads or writes of every item in the transaction: one to prepare the transaction and one to commit the transaction.

## SQS

<!-- prettier-ignore-start -->

|                            |                                                                                                                |
| -------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Message size               | Minimum message size is 1 byte. Maximum message size is 256 KB                                                 |
| Message visibility timeout | The default visibility timeout for a message is 30 seconds. The minimum is 0 seconds. The maximum is 12 hours. |
| Long polling wait time     | Maximum 20 seconds                                                                                             |
| Delay queue                | Default (minimum) delay for a queue is 0 seconds. The maximum is 15 minutes                                    |
{:.table-striped}

<!-- prettier-ignore-end -->

### FIFO

-   FIFO (First-In-First-Out) queues are designed to enhance messaging between applications when the order of operations and events is critical, or where duplicates can't be tolerated.

-   FIFO queues also provide exactly-once processing but have a limited number of transactions per second.

-   `MessageDeduplicationId` token used for deduplication of sent messages. If a message with a particular MessageDeduplicationId is sent successfully, any messages sent with the same MessageDeduplicationId are accepted successfully but aren't delivered during the 5-minute deduplication interval.

-   `MessageGroupId` tag specifies that a message belongs to a specific message group. Messages that belong to the same message group are processed in a FIFO manner (however, messages in different message groups might be processed out of order).

### Server-Side Encryption

-   SQS encrypt messages stored in both Standard and FIFO queues can be encrypted using KMS.

-   SSE encrypts the body of the message, but does not touch the queue metadata, the message metadata, or the per-queue metrics.

-   Adding encryption to an existing queue does not encrypt any backlogged messages.

## ELB

-   Application load balancer supports following protocols: `http, https, websockets`.

-   Application load balancer supports host and path based routing.

-   Because load balancers intercept traffic between clients and servers, your server access logs contain only the IP address of the load balancer. To see the IP address of the client, use the `X-Forwarded-For` request header.

## KMS

-   KMS can be used to decrypt/encrypt up to `4KB of data`.

<!-- prettier-ignore-start -->

|                          |                                                                                                                                                                                                                            |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AWS managed CMK          | You cannot manage key rotation for AWS managed CMKs. <br/>AWS KMS automatically rotates AWS managed CMKs every three years.                                                                                                   |
| Customer aws managed CMK | Automatic key rotation is disabled by default on customer managed CMKs. <br/>When you enable (or re-enable) key rotation, AWS KMS automatically rotates the CMK 365 days after the enable date and every 365 days thereafter. |
{:.table-striped}

<!-- prettier-ignore-end -->

### Envelope Encryption

If you need to encrypt more than 4 KB of data ,then you have to encrypt data locally in your application making use of envelope encryption.

1. Use the `GenerateDataKey` operation to get a data encryption key.

2. Use the plaintext data key (returned in the Plaintext field of the response) to encrypt data locally, then erase the plaintext data key from memory.

3. Store the encrypted data key (returned in the CiphertextBlob field of the response) alongside the locally encrypted data.

## IAM

### Policy Evaluation Logic

-   By default, all requests are implicitly denied.

-   An explicit deny in any policy overrides any allows and the final decision will be `deny`.

-   Organizations service control policies, Resource-based policies, IAM permissions boundaries, Session policies, Identity-based policies are then evaluated to determine whether to allow or deny the requests. If there are no statements that allow the requested action then the final decision will be `deny`.

## Kinesis

Kinesis is composed of stream, firehose and analytics components.

Kinesis Data Streams - Amazon Kinesis Data Streams is a scalable and durable real-time data streaming service that can continuously capture gigabytes of data per second from hundreds of thousands of sources.

Kinesis Firehose - Amazon Kinesis Data Firehose is the easiest way to capture, transform, and load data streams into AWS data stores for near real-time analytics with existing business intelligence tools.

Kinesis Analytics - Amazon Kinesis Data Analytics is the easiest way to process data streams in real time with SQL or Java without having to learn new programming languages or processing frameworks.

### Kinesis Data Streams

-   A data stream represents a group of data records. The data records in a data stream are distributed into shards.

-   A shard has a sequence of data records in a stream.

-   Records are ordered per shard basis.

-   Partition key is used to segregate and route records to different shards of a data stream. A partition key is specified by your data producer while adding data to an Amazon Kinesis data stream.

<!-- prettier-ignore-start -->

|                             |                   |
| --------------------------- | ----------------- |
| Data retention period       | 24 hours - 7 days |
| Maximum size of a data blob | 1 MB              |
| Shard data input capacity   | 1 MB/sec          |
| Shard data output capacity  | 2 MB/sec          |
{:.table-striped}

<!-- prettier-ignore-end -->

#### Writing Data

##### Kinesis Producer Library

`Kinesis Producer Library (KPL)` simplifies producer application development, allowing developers to achieve high write throughput to a Kinesis data stream.

KPL performs these tasks to achieve high write throughput to a kinesis stream.

-   Writes to one or more Kinesis data streams with an automatic and configurable retry mechanism

-   Collects records and uses PutRecords to write multiple records to multiple shards per request

-   Aggregates user records to increase payload size and improve throughput

-   Integrates seamlessly with the Kinesis Client Library (KCL) to de-aggregate batched records on the consumer

-   Submits Amazon CloudWatch metrics on your behalf to provide visibility into producer performance

#### Reading Data

##### Kinesis Client Library

The Kinesis Client Library (KCL) helps you consume and process data from a Kinesis data stream.

The KCL takes care of many of the complex tasks associated with distributed computing, such as load balancing across multiple instances, responding to instance failures, checkpointing processed records, and reacting to resharding.

-   Each shard can be read by only one KCL instance.

-   For each Amazon Kinesis Data Streams application, the KCL uses a unique Amazon DynamoDB table to keep track of the application's state.

-   Kinesis Client Library (KCL) delivers all records for a given partition key to the same record processor, making it easier to build multiple applications reading from the same Amazon Kinesis data stream (for example, to perform counting, aggregation, and filtering)

{% include donate.html %}
{% include advertisement.html %}

## API Gateway

### Lambda Authorizers for API

-   A Lambda authorizer (formerly known as a custom authorizer) is an API Gateway feature that uses a Lambda function to control access to your API.

-   A Lambda authorizer is useful if you want to implement a `custom authorization scheme` that uses a bearer token authentication strategy such as OAuth or SAML, or that uses request parameters to determine the caller's identity.

### IAM authentication for API

To enable IAM authentication for your API:

1. Under Settings, for Authorization, choose `AWS_IAM`.

2. Grant API authorization to a group of IAM users i.e. add users to an IAM group and attach policies with required permissions to the group.

3. Authenticate requests that are sent to API Gateway using `Signature Version 4` signing process.

### Caching

-   To invalidate API gateway cache entry and reload it from the integration endpoint, the client must send a request that contains `Cache-Control: max-age=0` header

<!-- prettier-ignore-start -->

|                                   |                   |
| --------------------------------- | ----------------- |
| Default TTL value for API caching | 300 seconds       |
| Maximum TTL value for API caching | 3600 seconds      |
| Supported Cache size              | 0.5GB up to 237GB |
{:.table-striped}

<!-- prettier-ignore-end -->

## Cloudwatch

## Cloudformation

### Intrinsic Functions

<!-- prettier-ignore-start -->

|     |                                                                                                                                                                                                                                                                                                |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ref | Returns the value of the specified parameter or resource<br/>When you specify a parameter's logical name, it returns the value of the parameter<br/>When you specify a resource's logical name, it returns a value that you can typically use to refer to that resource, such as a physical ID |
|Fn::GetAtt| Returns the value of an attribute from a resource in the template <br/>e.g. "Fn::GetAtt" : [ "myELB" , "DNSName" ]<br/>Returns a string containing the DNS name of the load balancer with the logical name myELB|
|Fn::FindInMap|Returns the value corresponding to keys in a two-level map that is declared in the Mappings section<br/>e.g. { "Fn::FindInMap" : [ "MapName", "TopLevelKey", "SecondLevelKey"] }|
|Fn::Join|Appends a set of values into a single value, separated by the specified delimiter|
|Fn::Sub|Substitutes variables in an input string with values that you specify|
{:.table-striped}

<!-- prettier-ignore-end -->

## Cognito

<!-- prettier-ignore-start -->

|                                               |                                                                                                                                                                                                                                                                   |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Cognito User Pool                             | User pools are for authentication (identify verification). <br/>User Pools provide a user directory for your application like sign-up, sign-in, group management, etc. (or) use identity providers, such as Facebook or Google.                                                                                                   |
| Cognito Federated Identities or Identity Pool | Identity pools are for authorization (access control).<br/>With a federated identity, you can obtain temporary, limited-privilege AWS credentials to securely access other AWS services such as Amazon DynamoDB, Amazon S3, and Amazon API Gateway. <br/>Identity Pools map a user from an Identity Provider to an IAM role. |
{:.table-striped}

<!-- prettier-ignore-end -->

## X-RAY

With X-Ray, you can understand how your application and its underlying services are performing to identify and troubleshoot the root cause of performance issues and errors.

Segments - The compute resources running your application logic send data about their work as segments. A segment provides the resource's name, details about the request, and details about the work done. e.g. host, request, response, start and end times.

Sampling - To ensure efficient tracing and provide a representative sample of the requests that your application serves, the X-Ray SDK applies a sampling algorithm to determine which requests get traced.

<!-- prettier-ignore-start -->

| Annotations                                            | Metadata                                                                      |
| ------------------------------------------------------ | ----------------------------------------------------------------------------- |
| Key-value pairs with string, number, or Boolean values | Key-value pairs that can have values of any type, including objects and lists |
| Indexed for use with filter expressions                | Not indexed for use with filter expressions                                   |
{:.table-striped}

<!-- prettier-ignore-end -->

## Step Functions

-   Maximum execution time -> `1 year`

{% include donate.html %}
{% include advertisement.html %}

## SAM

-   The AWS Serverless Application Model (SAM) is an open-source framework for building serverless applications.

-   You use the AWS SAM specification to define your serverless application.

-   The declaration `Transform: AWS::Serverless-2016-10-31` is required for AWS SAM templates.

-   `Resources` section is required for AWS SAM templates.

-   Some of the supported SAM resource and property types - `AWS::Serverless::Api, AWS::Serverless::Function, AWS::Serverless::SimpleTable`.

-   After you develop and test your serverless application locally, you can deploy your application by using the `sam package` and `sam deploy` commands.

-   sam package and sam deploy commands described in this section are identical to their AWS CLI equivalent commands `aws cloudformation package` and `aws cloudformation deploy`, respectively.

## CodeCommit

## CodeBuild

## CodeDeploy

## CodePipeline

## CodeStar

{% include donate.html %}
{% include advertisement.html %}

## References

<https://docs.aws.amazon.com/>
