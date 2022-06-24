---
layout: post
title: "FSx for NetApp ONTAP - Volume 'Tiering Policy Cooling Days' Parameter"
date: 2022-06-22 00:00:00 +0800
categories: fsxn volume-management
tags: fsx ontap netapp fsxn AWS Cloud
---

The common challenge when working with FSxN volumes using the AWS Management Console is ability to change the Cooling Period of the 'Auto' Tiering Policy.

Default value for Auto policy is 21 days. In order to change the value from AWS CLI use the following command:

```
aws fsx update-volume --volume-id VOL_ID --ontap-configuration TieringPolicy={CoolingPeriod=12}
```

> **Note**: Volume Id is the AWS identifier of the volume and not the UUID or Volume name
