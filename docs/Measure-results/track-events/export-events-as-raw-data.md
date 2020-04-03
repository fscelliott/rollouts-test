---
title: "Export events as raw data"
slug: "export-events-as-raw-data"
hidden: false
createdAt: "2019-09-11T11:47:40.660Z"
updatedAt: "2019-12-04T17:41:10.419Z"
---
Optimizely's data export services enable you to access all of your Optimizely event data. The services rely on a daily job that collects all events received in the last 24 hours for all experiments in your account. It then exports them securely to an S3 bucket, which can be programmatically accessed via Amazon's APIs with a secure set of credentials provided by Optimizely.

For more information about exporting Full Stack data, see:
 - [Access Optimizely raw data](https://help.optimizely.com/Analyze_Results/Access_Optimizely_raw_data) 
 - [Optimizely Data Export Services](https://developers.optimizely.com/x/events/export/index.html)