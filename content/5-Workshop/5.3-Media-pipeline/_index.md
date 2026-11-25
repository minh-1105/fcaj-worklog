---
title : "Media pipeline: S3, Lambda, and CloudFront"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

The media stack is deployed first because the API stack imports its outputs (bucket name, CloudFront URL, SNS topic).

#### How the pipeline works

1. The API checks the seller and returns a short-lived **presigned POST** for a fixed key under `raw/`.
2. The browser uploads the file directly to S3 — the image never passes through the API.
3. `s3:ObjectCreated` under `raw/` invokes the **ImageProcessor** Lambda.
4. ImageProcessor writes `large.webp` (1600px) and `thumbnail.webp` (480px) under `processed/`.
5. CloudFront reads **only** `processed/*` through Origin Access Control; `raw/` stays private and expires after 7 days.
6. After two failed retries, the event is sent to an encrypted **SQS** failure queue.

#### Contents

1. [Deploy the media stack](5.3.1-deploy-media-stack/)
2. [Test the image pipeline](5.3.2-test-pipeline/)
