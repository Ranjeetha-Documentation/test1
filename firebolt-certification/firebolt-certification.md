# Firebolt Certification

------------------------------------------------------------------------

# Introduction

This document provides a comprehensive guide for setting up the Firebolt Certification Suite (FCS), designed to enable both internal and external parties to verify that their applications comply with Firebolt standards. Additionally, it includes usage examples for employing FCS in conjunction with Mock Firebolt to facilitate an effective testing environment.

------------------------------------------------------------------------

# Prerequisites

Before proceeding with the setup of FCS, ensure the following prerequisites are met:

-   Node.js: A recent version of Node.js must be installed on your system. We recommend using version 20 or older. You can download it from
    [nodejs.org](https://nodejs.org/en)
    .
-   npm (Node Package Manager): This comes bundled with Node.js and will be used to install project dependencies.
-   yarn: Also a package manager for node. Good installation directions can be found
    [here](https://www.hostinger.com/tutorials/how-to-install-yarn)
    .
-   git: Required for cloning the repository. Git should come pre-installed on Mac and Linux. If you are on Windows, you can install git from
     
    [git-scm.com](https://git-scm.com/)
    .

------------------------------------------------------------------------

# Cloning the Repository

1.  Open a terminal on your system.
2.  Navigate to the directory where you want to clone the FCS repository.
3.  Run the following command to clone the repository:
    1.  git clone &lt;FCS-repository-url&gt;

------------------------------------------------------------------------

# Config Modules

**Understanding Config Modules:**

FCS can operate seamlessly without a config module for standard usage. However, the incorporation of a config module is encouraged when a user needs to tailor the testing environment to specific requirements or scenarios.

Config modules are essentially extensions that provide custom configuration settings for conducting certification tests on a designated platform. These modules are pivotal in scenarios where the standard testing protocol needs adaptation to suit particular platform intricacies.

Without a config module, FCS uses the default module to operate.

**Crafting Your Own Config Module for Mock Firebolt:**

Users will need to create their own config module for Mock Firebolt. To embark on creating a custom config module, the default module directory within the FCS repository offers a solid foundation. This template should be expanded on, and the user will need to provide their own PubSub system in order to utilize the App Transport layer of the application. Additional details about building out a Mock Firebolt config module are pending.

**What the Mock Firebolt Config Module Does:**

The Mock Firebolt Config Module for FCS should accomplish the following:

-   Establishes WebSocket connections for seamless communication with Mock Firebolt.
-   Manages WebSocket communications to ensure robust and reliable data exchange.
-   Dynamically launches third-party applications, utilizing environment variables to guide the process.

------------------------------------------------------------------------

# Installation with a Mock Firebolt Config Module

After cloning the repository, you need to configure your config module and install dependencies.

Navigate into the cloned repository:

```
$cd firebolt-certification-suite
```

Add your config module to the dependencies list within the package.json file.

```
"dependencies": &#123;
"configModule": "git+ssh://&lt;URL of Mock Firebolt Config Module&gt;",
&#125;
```

Run the following command to install the dependencies and your config module:

```
$npm install
```

------------------------------------------------------------------------

# Configuration

Environment variables inside of /cypress/support/common.js should be modified to fit your test needs. To run a sanity suite with Mock Firebolt, the following configuration variables are required:

<table class="wrapped">
<tbody>
<tr class="header">
<th><p><strong>Field</strong></p></th>
<th><p><strong>Type</strong></p></th>
<th><p><strong>Sample</strong>
<strong></strong>
<strong>Value</strong></p></th>
<th><p><strong>Description</strong></p></th>
</tr>
&#10;<tr class="odd">
<td><p>deviceMac</p></td>
<td><p>string</p></td>
<td><p>F0:AA:BB:EF:D1:C1</p></td>
<td><p>The Mac address of the device.</p></td>
</tr>
<tr class="even">
<td><p>mock</p></td>
<td><p>boolean</p></td>
<td><p>false</p></td>
<td><p>Whether to use a mock firebolt for testing. Default value is false. Need to set to true.</p></td>
</tr>
<tr class="odd">
<td><p>certification</p></td>
<td><p>boolean</p></td>
<td><p>true</p></td>
<td><p>Allows overriding excluded methods and modules from config module when certification is true.</p></td>
</tr>
<tr class="even">
<td><p>reportType</p></td>
<td><p>string</p></td>
<td><p>'cucumber' or 'mochawesome'</p></td>
<td><p>Defines the report type that will be generated by FCS. Set this to mochawesome when running the sanity suite.</p></td>
</tr>
</tbody>
</table>

**Configuration for Mock Firebolt Config Module**

Modifications to config module variables inside your config module may be necessary. Ensure variables are set with appropriate URLs, device settings, and application IDs.

Below are the environment variables that need to be modified exclusively for sanity runs:

```
"excludedMethods" : [] // Array of strings that includes excluded methods you want to skip over
"event_param": &#123; "listen": true &#125; // When a 1st party app registers for an event, if the params in the request are empty, it will be overriden with the event_param value.
```

------------------------------------------------------------------------

# Running FCS with Mock Firebolt

Before running FCS, you will need to have Mock Firebolt running. Please refer to the
[Mock Firebolt Documentation](https://github.com/rdkcentral/mock-firebolt)
 on how to get started. To initiate sanity test runs within FCS, utilize the command below:

```
npm run cy:open --e2e --browser electron --config-file cypress.fireboltCertification.js
```

 
**Execution Steps:**

-   The npm run cy:open command activates the Cypress interface in Electron browser mode.
-   Within the Cypress UI, navigate and select the desired sanity test suite to commence the test execution process.

------------------------------------------------------------------------

# Report Generation in FCS

After executing test cases in FCS, the system is configured to automatically create detailed reports in JSON format. You have the option of generating these reports either as mochawesome or cucumber, based on your preferences and requirements. Each format provides an in-depth analysis of the test results.

To tailor the reporting to your needs, you can adjust the reportType parameter within your Cypress configuration. Set this parameter to either cucumber or mochawesome, depending on which format you prefer. This configuration choice will generate an HTML version of the report, offering a user-friendly interface to visualize and interpret the test outcomes.

**Location of Reports:**

The generated reports are systematically stored within the ./reports/&#123;$jobId&#125; directory, where &#123;$jobId&#125; is a unique identifier for each test run. This organization ensures that you can easily locate and distinguish between reports from different test executions.

