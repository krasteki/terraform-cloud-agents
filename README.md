# Learn Terraform - Using Terraform Cloud Agents

This is a companion repository for the [Using Cloud Agents learn
guide](https://learn.hashicorp.com/tutorials/terraform/cloud-agents),
and contains Terraform conifguration files for you to use to learn how to
configure self-hosted Terraform Cloud Agents.


I. Create an agent pool

1. To create an agent pool, navigate to the "Agents" panel within Terraform Cloud's "Settings" page and click "Create agent pool."

2. Terraform Cloud will prompt to generate a token for the agent pool. Add agent1 as the description and click "Create token."

3. Configure  workspace to use agents

Workspace -> Settings-> General tab

III. Launch the agent

1. Create a `agent1.list` file with the `token` and `agent name`

TFC_AGENT_TOKEN=<YOUR TOKEN>
TFC_AGENT_NAME=agent1

2. Launch the agent
```
$ docker run --name tfc_agent --env-file agent1.list -v /var/run/docker.sock:/var/run/docker.sock hashicorp/tfc-agent:latest
```
mounting the Docker socket using `-v /var/run/docker.sock:/var/run/docker.sock`. Mounting the socket allows the containerized agent to use the Docker provider to manage other containers on the machine.

3. Open a new terminal and run:
```
$ docker exec -it -u 0 tfc_agent /bin/bash
```
Change the permissions on the Docker socket to grant the `tfc-agent` user read and write privileges.
```
$ chmod 666 /var/run/docker.sock
$ exit
```

IV. Create a local Nginx container

V. Configure a second agent

1. Create a `agent2.list` file with the `token` and `agent name`
2. From settings-> agents-> education (agent pool)-> create new token

TFC_AGENT_TOKEN=<YOUR TOKEN>
TFC_AGENT_NAME=agent2

3. Lunch the second agent
```
$ docker run --env-file agent2.list -v /var/run/docker.sock:/var/run/docker.sock hashicorp/tfc-agent:latest
```

VI. Destroy resources
VII. Disassociate the agent pool & Delete the agent pool