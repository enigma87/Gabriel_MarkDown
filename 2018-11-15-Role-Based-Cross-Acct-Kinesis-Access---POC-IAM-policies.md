---
title: Role Based Cross Acct Kinesis Access - POC IAM policies
layout: post
author: kishore.kailainathan
permalink: /role-based-cross-acct-kinesis-access---poc-iam-policies/
source-id: 1UAOqaq1pRFN5EBTzJRYr8fxM5xm8ywgvMk6hVuio6m8
published: true
---
**HMH **policies associated with a Kinesis consumer role

<table>
  <tr>
    <td>
arn:aws:iam::aws:policy/service-role/AWSLambdaKinesisExecutionRole# additional permissions for cloudwatch/queues

{    "Version": "2012-10-17",    "Statement": [        {            "Sid": "VisualEditor0",            "Effect": "Allow",            "Action": "sts:AssumeRole",            "Resource": "arn:aws:iam::RENACCTNUMBR:role/{hmh_kinesis_consumer_role}"        }    ]}</td>
  </tr>
</table>


**RENAISSANCE ** 

Trust Policy on IAM role hmh_kinesis_consumer_role for HMH Acct. The principal here will be more restrictive in practice, say an IAM role on HMH Acct.

<table>
  <tr>
    <td>
{  "Version": "2012-10-17",  "Statement": [    {      "Effect": "Allow",      "Principal": {        "AWS": "arn:aws:iam::HMHACCTNUMBR:root"      },      "Action": "sts:AssumeRole",      "Condition": {}    }  ]}
</td>
  </tr>
</table>


Permission associated with IAM role hmh_kinesis_consumer_role 

<table>
  <tr>
    <td>{    "Version": "2012-10-17",    "Statement": [        {            "Effect": "Allow",            "Action": [                "kinesis:Get*",                "kinesis:List*",                "kinesis:Describe*"            ],            "Resource": "arn:aws:kinesis:us-east-1:239641032376:stream/enhanced_data_stream"        }    ]}</td>
  </tr>
</table>


**Example AssumeRole, Kinesis iterator**

<table>
  <tr>
    <td>import loggingimport boto3import jsonimport datetimeimport osimport timelogger = logging.getLogger()logger.setLevel(logging.INFO)sts_client = boto3.client('sts')assumedRoleObject = sts_client.assume_role(	RoleArn="arn:aws:iam::239641032376:role/kk_hmh_kinesis_consumer",	RoleSessionName="AssumeRoleSession1")credentials = assumedRoleObject['Credentials']kinesis  = boto3.client(	'kinesis',	aws_access_key_id=credentials['AccessKeyId'],	aws_secret_access_key=credentials['SecretAccessKey'],	aws_session_token=credentials['SessionToken'],)def lambda_handler(event, context):	stream_desc = kinesis.describe_stream(StreamName='hmh_premium_data_share')	logger.info(stream_desc)	shard_id = stream_desc['StreamDescription']['Shards'][0]['ShardId']	now = datetime.datetime.now()	now_minus_30m = now - datetime.timedelta(minutes = 30)	iterator = kinesis.get_shard_iterator(    	StreamName='hmh_premium_data_share',    	ShardId= shard_id,    	ShardIteratorType='AT_TIMESTAMP',    	Timestamp=now_minus_30m)	shard_iterator = iterator['ShardIterator']	#logger.info(json.dumps(iterator))	response = kinesis.get_records(    	ShardIterator= shard_iterator)	logger.info(response['Records'])	while 'NextShardIterator' in response:    	    response = kinesis.get_records(ShardIterator=response['NextShardIterator'])    	    logger.info(json.dumps(response, default=date_str))    	    time.sleep(10)	return 'logged ren data event to cloudwatch!'      def date_str(o):	if isinstance(o, datetime.datetime):    	return o.__str__()</td>
  </tr>
</table>


