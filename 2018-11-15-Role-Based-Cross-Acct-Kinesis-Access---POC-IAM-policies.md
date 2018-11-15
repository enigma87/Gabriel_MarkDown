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
arn:aws:iam::aws:policy/service-role/AWSLambdaKinesisExecutionRole


  </tr>
</table>


**RENAISSANCE ** 

Trust Policy on IAM role hmh_kinesis_consumer_role for HMH Acct. The principal here will be more restrictive in practice, say an IAM role on HMH Acct.

<table>
  <tr>
    <td>
{
</td>
  </tr>
</table>


Permission associated with IAM role hmh_kinesis_consumer_role 

<table>
  <tr>
    <td>{
  </tr>
</table>


**Example AssumeRole, Kinesis iterator**

<table>
  <tr>
    <td>import logging
  </tr>
</table>

