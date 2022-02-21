# Google Summer of Code 2022 Ideas

## About LocalStack

LocalStack provides an easy-to-use test/mocking framework for developing Cloud applications. It spins up a testing environment on your local machine that provides the same functionality and APIs as a real cloud environment. With LocalStack, you can entirely develop on the local machines, where we provision all required "cloud" resources in a local container. LocalStack provisions all required "cloud" resources in the continuous integration server environment where changes are pushed to run the automated tests.

LocalStack aims to be a lightweight, easy-to-use, and flexible testing framework that can be used to test cloud-based applications. It helps teams and developers across the world to work on extremely efficient development and testing loops. We are proposing ideas for the Google Summer of Code program where students/professionals can apply to participate and work with us to help us achieve this mission.

## Application process

To get started with our application process, please read about [Google Summer of Code](https://summerofcode.withgoogle.com/) and the [Student Guide](https://google.github.io/gsocguides/student/). Make sure to go through all the sections on the left menu, though we ideally would like you to focus on **Introduction** & **Applying** sections to better understand the process.

Next, join our [Slack workspace](https://join.slack.com/t/localstack-community/shared_invite/zt-10v01mnte-tHiu0CTFdaGHYomID3WNcQ) and introduce yourself on the `#community` channel. Through the Slack workspace, you can get in touch with other users, contributors and mentors in the community. Before discussing the project idea, please make sure:

- You have a good understanding of the technologies you will be working with, if you are selected to participate in GSoC.
- You agree with our [Code of Conduct](https://github.com/localstack/.github/blob/main/CODE_OF_CONDUCT.md) and [Slack guidelines](https://github.com/localstack/.github/blob/main/SLACK_GUIDELINES.md).
- You agree to commit to the timeline of the program, write weekly blogs/reports on your project and engage with the wider LocalStack community.

After a discussion with the mentors, you will be required to write a proposal for the project. GSoC guide provides a [writing a proposal guide](https://google.github.io/gsocguides/student/writing-a-proposal) for students. We have also provided our own [GSoC template](../GSOC-PROPOSAL-TEMPLATE.md) that you can use as a starting point. Share the proposal for review over Google Docs, HackMD, or any medium you prefer to share your proposal with us (without ideally making us download anything). After an initial review, you can submit your proposal to the [Google Summer of Code](https://summerofcode.withgoogle.com/) website once the applications are open.

## Ideas

### Idea: Standalone Command-Line interface

#### Abstract

Extract the CLI from the core codebase and investigate different packaging and distribution options.

| Category              | Rating                                                                                       |
| --------------------- | -------------------------------------------------------------------------------------------- |
| Type                  | Peripheral                                                                                   |
| Size                  | Medium                                                                                       |
| Preferred contributor | Student/Professional                                                                         |
| Mentors               | [@dfangl](https://github.com/dfangl), [@dominikschubert](https://github.com/dominikschubert) |
| Skills                | Python, Docker                                                                               |

#### Description

LocalStack’s CLI has multiple responsibilities. It works both as a tool to send lifecycle commands (i.e. starting or stopping LocalStack) and as a tool to display logs or other debugging information about the actively running instance.

Historically it was and still is very tightly coupled to the core LocalStack's Python codebase. This has gotten better, but there are still many issues with having no proper separation between these two. There is no client specification, so clients would all have to delegate to the CLI.

Another drawback of the current setup is that releases of the CLI are coupled with [releases to LocalStack](https://pypi.org/project/localstack/) itself. Because the CLI is exclusively distributed via `pip`, it forces users to install Python and `pip` as well and needs to pull in a lot of additional dependencies. This also breaks in some situations where version conflicts arise on the host system.

In the course of this project different packaging and distribution options should be evaluated via implementations. A control plane (via an HTTP API) should be designed and implemented in LocalStack with a corresponding CLI client. This client should then ultimately be distributed through one of the previously evaluated methods.

#### Resources

- [LocalStack](https://github.com/localstack/localstack)
- [LocalStack CLI](https://docs.localstack.cloud/get-started/#localstack-cli)
- [LocalStack package](https://pypi.org/project/localstack/)

### Idea: Hybrid Cloud Setups with LocalStack

#### Abstract

Connecting local development environments to real cloud resources via hybrid setups.

| Category              | Rating                                                                     |
| --------------------- | -------------------------------------------------------------------------- |
| Type                  | Peripheral/Core Development                                                |
| Size                  | Large                                                                      |
| Preferred contributor | Student/Professional                                                       |
| Mentors               | [@whummer](https://github.com/whummer), [@thrau](https://github.com/thrau) |
| Skills                | Python, Docker, AWS                                                        |

#### Description

LocalStack provides a cloud-like environment that can be used for development and testing without needing the actual cloud available. However we are looking into the options of connecting our local development & testing environments to real cloud resources using hybrid setups.

The project would entail creating proxy mechanisms for some key services (e.g., DynamoDB, S3, SQS), to allow a “local view” of a subset of the data contained in the real cloud resource. The project would also involve finding a mechanism for managing authentication - which credentials to use, how to configure them securely, and more.

We will be using AWS services predominantly and the project would involve the necessity to introduce callbacks from real AWS into the local dev environment (e.g., an event is received in an SQS queue in the real cloud, then a local Lambda function should be triggered).

This will allow having quick feedback cycles (and easy debuggability) for local representations of cloud resources, while still keeping some of the heavy liftings in the real cloud (hence the term “hybrid” setup).

#### Resources

- [S3Proxy](https://github.com/gaul/s3proxy): Proxy server to access different storage backend servers like S3.
- [AWS SigV4 Proxy](https://github.com/awslabs/aws-sigv4-proxy): Generic reverse proxy to access AWS resources.
- [dynamo-cassandra-proxy](https://github.com/datastax/dynamo-cassandra-proxy): Proxying DynamoDB workloads to Cassandra backend.

### Idea: Cloud Service Emulation in Google Cloud Platform

#### Abstract

Cloud service emulation in Google Cloud Platform (GCP).

| Category              | Rating                                                                           |
| --------------------- | -------------------------------------------------------------------------------- |
| Type                  | Peripheral/Exploratory                                                           |
| Size                  | Large                                                                            |
| Preferred contributor | Student                                                                          |
| Mentors               | [@alexrashed](https://github.com/alexrashed), [@thrau](https://github.com/thrau) |
| Skills                | Python, Docker, GCP                                                              |

#### Description

LocalStack is currently primarily tailored to emulate Amazon Web Services (AWS), but we are looking to integrate other cloud providers such as Google Cloud Platform (GCP). LocalStack leverages API specifications, code generation, and existing third-party cloud emulators, to create a coherent local cloud stack.

To expand to GCP, we first need to explore the space, specifically the tools to work with GCP API specifications (that are often found as part of client software), as well as existing service-specific GCP emulators. The goal of this project is to use the techniques we apply to emulate AWS to build a project for emulating GCP. This will involve:

1. Tech spikes into existing GCP cloud service emulators to understand their scope and technology
2. Investigate GCP API specifications and tools to work with them
3. Work with a core developer member to generate server-side API stubs for GCP.
4. Hide existing GCP emulators behind the generated APIs. 
5. Write at least one original GCP service emulator.

The broader goal is also to understand the differences between emulating GCP and AWS, and use those insights to drive changes within the LocalStack codebase.

#### Resources

- [Google API specifications](https://github.com/googleapis/googleapis)
- [Python client to work with Google APIs](https://github.com/googleapis/google-api-python-client)
- [Existing GCP emulators](https://cloud.google.com/sdk/gcloud/reference/beta/emulators)
- [LocalStack’s framework for AWS](https://github.com/localstack/localstack/tree/master/localstack/aws)

### Idea: Implementing Azure API specifications for server-side code generation and routing in LocalStack

#### Abstract

Implementation of Azure API specifications for server-side code generation and routing in LocalStack.

| Category              | Rating                                                                                             |
| --------------------- | -------------------------------------------------------------------------------------------------- |
| Type                  | Exploratory                                                                                        |
| Size                  | Medium                                                                                             |
| Preferred contributor | Student/Professional                                                                               |
| Mentors               | [@alexrashed](https://github.com/alexrashed), [@viren-nadkarni](https://github.com/viren-nadkarni) |
| Skills                | Python, Docker, Azure                                                                              |

#### Description

At LocalStack, we built a framework - called the AWS Service Framework, or ASF. It uses the official machine-readable specification of AWS services to take care of the parsing and serialization, as well as to generate server-side code. The goal is to eliminate any kind of boilerplate code. LocalStack contributors should be able to concentrate on implementing the actual business logic, not all the boilerplate code besides that.

When starting to support Microsoft Azure, we want to use automated tools from the beginning. This project aims at exploring the Azure cloud, its SDKs, and its machine-readable specifications.

After the initial research phase, the implementation should use these specifications to generate server-side code (interfaces), start an API server, and parse incoming requests, call the generated server-side code, and serialize the response. The project can then led to the implementation of first service by integrating one of the publicly available emulators (like Azurite, or the Cosmos DB emulator).

#### Resources

- [Azure REST API Specifications](https://github.com/Azure/azure-rest-api-specs): This repository contains Swagger (”predecessor” of OpenAPI) with machine readable specifications of all Azure APIs.
- [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python)
- [Azurite](https://github.com/azure/azurite)
- [Cosmos DB emulator](https://docs.microsoft.com/en-us/azure/cosmos-db/linux-emulator)

### Idea: Extend AWS Account-Services

#### Abstract

Extend LocalStack with (account) management services such as "AWS Billing and Cost Management” or "AWS Organizations" and AWS Account Management Documentation.

| Category              | Rating                                                 |
| --------------------- | ------------------------------------------------------ |
| Type                  | Core Development                                       |
| Size                  | Medium                                                 |
| Preferred contributor | Student/Professional                                   |
| Mentors               | [@dominikschubert](https://github.com/dominikschubert) |
| Skills                | Python, Docker, AWS                                    |

#### Description

Testing scripts and other automations for AWS account services is not trivial in practice, because cleanup is a very tedious and slow process. Additionally, documentation is often lacking. In this project we want to extend our emulation layer with support for these APIs and integrate them with the existing services in LocalStack.

The project will involve:

- Implementing Billing and Cost Management APIs and related integrations such as Budget alerts via SNS.
- Implementing Organizations/Account API to create new accounts and organizations, manage multiple Accounts via the Organization and more.
- Implementing AWS Single-Sign-On and related authentication mechanisms to interact with LocalStack Services using these SSO credentials.

#### Resources

- [AWS Organizations API Reference](https://docs.aws.amazon.com/organizations/latest/APIReference/Welcome.html)
- [AWS Cost Explorer](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/Welcome.html)
- [AWS Single Sign-On Documentation](https://docs.aws.amazon.com/singlesignon/?id=docs_gateway)
