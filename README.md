# Python Durable Functions Preview

Follow this guide to get up and running with Python Durable Functions!

> **Note:** (2020-03-13) Because updated versions of the Azure Functions Python worker, extension bundles, and templates that are needed for the preview have not yet been published or deployed to Azure, you need to use a dev container with Visual Studio Code Remote for Containers or Visual Studio Online.

## Prerequisites

* Azure subscription - Durable Functions requires a storage account. The easiest way to get started is by connecting to a storage account in Azure.
* An editor that supports Visual Studio dev containers
    - Visual Studio Code
        - Docker - the local machine must be able to run Linux Docker containers
        - [Remote Development extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) - starts and connects to the dev container
    - Visual Studio Online
        - VS Online provides a development environment in the cloud that you can connect to using VS Code or VS Online's full-featured, browser-based environment. 

## Instructions

### Start the development environment

#### Visual Studio Code with Remote Development extension and Docker

1. Clone this repo to your computer.
1. Open the repo's folder in VS Code.
1. Using the Command Palette, search for and run the *Reopen in Container* command. VS Code will reopen, start the development environment in Docker, and connect to it.

#### Visual Studio Online

1. Click this magical button: [Open in Visual Studio Online](https://online.visualstudio.com/environments/new?name=Python+Durable+Functions+Preview&repo=https://github.com/anthonychu/python-durable-preview)
1. If prompted, sign in.
1. A Create Environment dialog appears. Confirm the information and click Create.

### Create the Azure Functions project

The preconfigured development environment includes the Azure Functions Core Tools CLI (`func`)with the preview Python lanugage worker, useful VS Code extensions, as well as templates to help you get started.

1. In the editor, you should have the default workspace folder opened.
1. Using the Command Palette, search for and run the *Azure Functions: Create New Project...* command.
1. Select the following responses when prompted:
    | Prompt | Value | Description |
    | --- | --- | --- |
    | Specify a folder | Current open folder | |
    | Select a language | Python | |
    | Python version | Python 3.8 | Azure Functions supports Python 3.6 and above |
    | Select a template | Skip for now | |

### Activate virtual environment and install dependencies

When you created the project, the Azure Functions VS Code extension automatically creates a virtual environment with your selected Python version. You will activate the virtual environment in a terminal and install some dependencies required by Azure Functions and Durable Functions.

1. Open the editor's integrated terminal in the current folder (`` ctrl-` ``).
1. In the integrated terminal, activate the virtual environment in the current folder:
    ```bash
    source .venv/bin/activate
    ```
1. Open `requirements.txt` in the editor change its content to the following:
    ```
    azure-functions>=1.2.0
    azure-functions-durable>=1.0.1b2
    ```
1. Use pip to install the packages we just defined:
    ```bash
    python -m pip install requirements.txt
    ```

Use this integrated terminal with the activated virtual environment for the rest of this tutorial.

### Create the functions

The most basic Durable Functions app contains three functions:
- *Orchestrator function* - describes a workflow that orchestrates other functions
- *Activity function* - called by the orchestrator function, performs work, and optionally returns a value
- *Client function* - a regular Azure Function that starts an orchestrator function

#### Orchestrator function

1. In the integrated terminal, create a durable orchestration function Azure Functions Core Tools CLI:
    1. Run `func new`.
    1. Select *Durable Functions orchestrator*.
    1. Use the default name of *DurableFunctionsOrchestrator*.

This creates a function in a folder named *DurableFunctionsOrchestrator*. In the folder, you'll find a `function.json` file that contains metadata describing the function.

You'll also find the function in `__init__.py`. An orchestrator is a Python generator function that describes how activity functions are called.

#### Activity function

1. In the integrated terminal:
    1. Run `func new`.
    1. Select *Durable Functions activity*.
    1. Use the default name of *Hello*.

This creates a function in a folder named *Hello*. In the folder, you'll find a function that simply returns a greeting. An activity function is a normal Azure Function; it is where you'll do actual work such as accessing databases and perform calculations.

#### HTTP triggered client function

1. In the integrated terminal:
    1. Run `func new`.
    1. Select *Durable Functions HTTP starter*.
    1. Use the default name of *DurableFunctionsHttpStart*.

This creates a function in a folder named *Hello*. In the folder, you'll find a typical HTTP triggered Azure Function that also takes an orchestration client input binding. The function uses the orchestration client to start an orchestration and return an HTTP response containing URLs the caller can use to check the orchestration's status.

### Run the app

1. Press `F5` or select *Debug: Start debugging* from the Command Palette. The function app will start and the debugger will attach.