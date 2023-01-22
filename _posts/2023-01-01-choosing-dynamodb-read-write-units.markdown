---
layout: post
title:  "Brief Strategy for defining the Read/ Write Capacity Units for Provisioned DynamoDB"
date:   2023-01-01 00:18:23 +0700
categories: [aws, dynamodb]
---

## Introduction

DynamoDB comes with two options for choosing the dynamo instances:
    - on demand mode 
    - provisioned mode

On demand mode is recommended to be used in use cases in which a workload prediction is not possible or the application traffic cannot be counted.

The provisioned mode is used in the opposite cases, when both the workload and the traffic can be predicted or ramp gradually. This options allows forecasting capacity requirements to control costs.

## The Read Write Capacity Mode Definition

[AWS Read/write capacity mode](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html)

Important take away from the AWS documentation is how the read write units get consumed, aka how AWS does the cost calculation.

The following are taken from the AWS reference and are key in calculating the predictions for capacity

- **One read capacity unit** represents one strongly consistent read per second, or two eventually consistent reads per second, for an item up to 4 KB in size. Transactional read requests require two read capacity units to perform one read per second for items up to 4 KB. If you need to read an item that is larger than 4 KB, DynamoDB must consume additional read capacity units. The total number of read capacity units required depends on the item size, and whether you want an eventually consistent or strongly consistent read. For example, if your item size is 8 KB, you require 2 read capacity units to sustain one strongly consistent read per second, 1 read capacity unit if you choose eventually consistent reads, or 4 read capacity units for a transactional read request. For more information, see [Capacity unit consumption for reads](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughput.html#ItemSizeCalculations.Reads).

- **One write capacity unit** represents one write per second for an item up to 1 KB in size. If you need to write an item that is larger than 1 KB, DynamoDB must consume additional write capacity units. Transactional write requests require 2 write capacity units to perform one write per second for items up to 1 KB. The total number of write capacity units required depends on the item size. For example, if your item size is 2 KB, you require 2 write capacity units to sustain one write request per second or 4 write capacity units for a transactional write request. For more information, see [Capacity unit consumption for writes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughput.html#ItemSizeCalculations.Writes).

**If your application reads or writes larger items (up to the DynamoDB maximum item size of 400 KB), it will consume more capacity units.**

## Calculating Write Units

Understanding infrastructure of your service and the query actions strategy is key in calculating expected load.
That means thinking about both the Functional and the Non Functional Requirements which may impact the cost overall. What are the expected load volumes per second? Hoes does the system scale will have impact in the final estimations.


*Use Case Scenario:*

In the context of using [Lambda with MSK](https://docs.aws.amazon.com/lambda/latest/dg/with-msk.html) and dropping messages in batches in a dynamodb table, the key metrics to understand would be:

    1. how many partitions does the kafka topic have?

    2. depending on the answer to the question 1, we will have maximum the number of lambda instances being run in parallel equal to it

    3. how does the lambda function write the message? if for example the answer is in batches then the following point is important as well

    4. what is the size of a message? as per above, if total amount exceeds the 4kb limit, more units would be counted with each write


*Example Calculation:*

    Partitions: 3
    Lambda: writes in batches of 50
    Message size: 300 bytes

    With this we will have at peak a maximum of: 

        - 3 writes
        - in size of: 3*50*300/1000 = 45kb 
        - which would be counted as **45 write units**

## Calculating Read Units

Calculating read units goes the same way.  It is important to understand what are the expected high peak traffic values, how many concurrent accesses per second there will be and how will the data be retrieved. Besides understanding the message size, if items will be fetched 1 by 1 or in a list, will count differently at the end.

*Example Calculation:*

    Concurrent Reads / second: 100
    Message Size: 300 bytes
    Messages are fetched as lists, with max of 20

    maximum would then be: 100 * 20 * 300 / 1000 = 600 kb / 4kb = 150 read units

## Additional Resources

[Pricing calculator per units ](https://dynobase.dev/dynamodb-pricing-calculator/)

[AWS Price Calculator](https://calculator.aws/#/addService)