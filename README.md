# Python Durable Functions Preview 🐍🌩

Follow this guide to get up and running with Python Durable Functions!

> **Note:** (2020-03-13) Because updated versions of the Azure Functions Python worker, extension bundles, and templates that are needed for the preview have not yet been published or deployed to Azure, you need to use a dev container with Visual Studio Code Remote for Containers or Visual Studio Online.
>
> You'll be able to deploy to Azure in April.

## Prerequisites

* Azure subscription - Durable Functions requires a storage account. The easiest way to get started is by connecting to a storage account in Azure.
* An editor that supports Visual Studio dev containers
    - Visual Studio Code
        - Docker - the local machine must be able to run Linux Docker containers
        - [Remote Development extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) - starts and connects to the dev container
    - Visual Studio Online
        - VS Online provides a development environment in the cloud that you can connect to using VS Code or VS Online's full-featured, browser-based code editor. 

## Instructions

### Start the development environment

Choose one of the following options and follow the instructions to create a development environment.

#### Option 1 - Visual Studio Code with Remote Development extension and Docker

1. Clone this repo to your computer.

1. Open the repo's folder in VS Code.

1. Using the Command Palette (press `F1` or `Ctrl-Shift-P` or `Cmd-Shift-P` (macOS)), search for and run the *Reopen in Container* command. VS Code will reopen, start the development environment in Docker, and connect to it.

#### Option 2 - Visual Studio Online

1. In a new browser window, open this magical link: [Open in Visual Studio Online](https://online.visualstudio.com/environments/new?name=Python+Durable+Functions+Preview&repo=https://github.com/anthonychu/python-durable-preview)

1. If prompted, sign in.

1. A Create Environment dialog appears. Confirm the information and click *Create*. Wait a few minutes to create the environment.

1. Click *Connect* to open the VS Online in-browser code editor.
    ![VS Online editor](.devcontainer/media/vsonline-editor.png)

> *Note:* If you'd like to connect to this VS Online environment with VS Code on your computer instead of the in-browser editor, ensure that you have installed and signed in to the [VS Online extension](https://marketplace.visualstudio.com/items?itemName=ms-vsonline.vsonline), then click the *Open in Desktop* button on the bottom left of the browser.

### Create the Azure Functions project

The preconfigured development environment includes the Azure Functions Core Tools CLI (`func`) with the preview Python lanugage worker, useful VS Code extensions, as well as templates to help you get started.

1. In the editor, you should have the default workspace folder opened.

1. Press `F1` or `Ctrl-Shift-P` or `Cmd-Shift-P` (macOS) to open the Command Palette.

1. Search for and run the *Azure Functions: Create New Project...* command.
    > If you are using VS Online, the extensions may not load immediately the first time. If you don't see any Azure Functions commands, try reloading your browser.

    ![Create function app](.devcontainer/media/create-function-app.png)

1. Select the following responses when prompted:

    | Prompt | Value | Description |
    | --- | --- | --- |
    | Specify a folder | Current open workspace folder | |
    | Select a language | Python | |
    | Python version | Python 3.7 | Azure Functions supports Python 3.6 - 3.8 |
    | Select a template | Skip for now | |

### Activate virtual environment and install dependencies

When you created the project, the Azure Functions VS Code extension automatically created a virtual environment with your selected Python version. You will activate the virtual environment in a terminal and install some dependencies required by Azure Functions and Durable Functions.

1. Open `requirements.txt` in the editor change its content to the following:

    ```
    azure-functions>=1.2.0
    azure-functions-durable>=1.0.0b4
    ```

    Durable Functions requires `azure-functions` version 1.2.0 or greater.

1. Open the editor's integrated terminal in the current folder (`` Ctrl-Shift-` ``).

1. In the integrated terminal, activate the virtual environment in the current folder:

    ```bash
    source .venv/bin/activate
    ```

    ![Activate virtual environment](.devcontainer/media/activate-venv.png)

1. In the integrated terminal where the virtual environment is activated, use pip to install the packages we just defined:

    ```bash
    python -m pip install -r requirements.txt
    ```

Use this integrated terminal with the activated virtual environment for the rest of this tutorial.

### Create the functions

The most basic Durable Functions app contains three functions:
- *Orchestrator function* - describes a workflow that orchestrates other functions
- *Activity function* - called by the orchestrator function, performs work, and optionally returns a value
- *Client function* - a regular Azure Function that starts an orchestrator function

> *Note:* If you prefer the command line, you can also perform the steps below using Azure Functions Core Tool's `func new` command.

#### Orchestrator function

1. In the VS Code Command Palette (`F1` or `Ctrl/Cmd-Shift-P`), search for and run the *Azure Functions: Create Function...* command.

1. Following the prompts, provide the following values:
    | Prompt | Value | Description |
    | ------ | ----- | ----------- |
    | Select a template for your function | Durable Functions orchestrator | Create a Durable Functions orchestration |
    | Provide a function name | HelloOrchestrator | Name of your durable function |

> You may be prompted to install a linter and/or enable IntelliCode. Neither of these are required for this tutorial, but you may click *Install* and *Enable it and Reload Window*, respectively, if you wish.

This creates a function in a folder named *DurableFunctionsOrchestrator*. In the folder, you'll find a `function.json` file that contains metadata describing the function.

You'll also find the function in `__init__.py`. An orchestrator is a Python generator function that describes how activity functions are called.

#### Activity function

1. In the Command Palette, search for and run the *Azure Functions: Create Function...* command.

1. Following the prompts, provide the following values:
    | Prompt | Value | Description |
    | ------ | ----- | ----------- |
    | Select a template for your function | Durable Functions activity | Create an activity function |
    | Provide a function name | Hello | Name of your activity function |

This creates a function in a folder named *Hello*. In the folder, you'll find a function that simply returns a greeting. An activity function is a normal Azure Function; it is where you'll do actual work such as accessing databases and perform calculations.

#### HTTP triggered client function

1. Again, in the Command Palette, search for and run the *Azure Functions: Create Function...* command.

1. Following the prompts, provide the following values:    | Prompt | Value | Description |
    | Prompt | Value | Description |
    | ------ | ----- | ----------- |
    | Select a template for your function | Durable Functions HTTP starter | Create an HTTP starter function |
    | Provide a function name | DurableFunctionsHttpStart | Name of your activity function |
    | Authorization level | Anonymous | For demo purposes, allow the function to be called without authentication |

This creates a function in a folder named *DurableFunctionsHttpStart*. In the folder, you'll find a typical HTTP triggered Azure Function that also takes an orchestration client input binding. The function uses the orchestration client to start an orchestration and return an HTTP response containing URLs the caller can use to check the orchestration's status.

### Run the app

1. Press `F5` or select *Debug: Start Debugging* from the Command Palette. The function app will start and the debugger will attach.

1. Because no storage account was set in `local.settings.json`, the Azure Functions VS Code extension should prompt you to select a storage account. Sign in to Azure and create a new storage account. You may also select an existing storage account that you have *not* used with Durable Functions.
    ![Select storage account](.devcontainer/media/select-storage.png)

1. Once the app is started, click on the *Remote Explorer* icon in the VS Code activity bar.

1. Under *Environment Details*, port *7071* should already be listed as forwarded. Right-click on it and select *Copy Port URL*.
    ![Copy URL](.devcontainer/media/copy-url.png)

    > *Note:* If you're doing this in the browser version of VS Online and the copy function is not working, refresh the browser and try again.

1. Open another browser window and paste in the copied port URL. A default Function App page should display.
    ![Homepage](.devcontainer/media/homepage.png)

1. In the location bar, append `/api/orchestrators/DurableFunctionsOrchestrator` to the URL to trigger the *DurableFunctionsHttpStart* function and start an instance of *DurableFunctionsOrchestrator*.
    ![Start orchestration response](.devcontainer/media/http-response.png)

1. The HTTP function should return a set of URLs. Open the `statusQueryGetUri` in a browser window to view the orchestrator function's status.
    > In VS Online, localhost URLs may be returned. Replace `http://localhost` with the VS Online port URL.
    ![Orchestration status](.devcontainer/media/orchestration-status.png)

1. Stop debugging by clicking the red *Disconnect* button or press `Shift-F5`.

Congratulations! You've built and ran your first Python Durable Functions app! 🎉

## Next steps

* [Check out Durable Functions docs](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=python) (Python code samples coming soon)
* Send us feedback at our GitHub repo (coming really soon)
* Deploy the app to Azure (coming soon, bits should be deployed to Azure by end of March)
