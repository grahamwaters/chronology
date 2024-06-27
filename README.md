
# Chronology
<!-- [![Build Status](https://github.com/OthersideAI/chronology/actions/workflows/build.yml/badge.svg)](https://github.com/OthersideAI/chronology/actions/workflows/build.yml) -->
[![PyPI version](https://badge.fury.io/py/chronological.svg)](https://badge.fury.io/py/chronological)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python Versions](https://img.shields.io/pypi/pyversions/chronological.svg)](https://pypi.org/project/chronological/)

Chronology is a powerful library designed to simplify the development of complex language-powered applications using OpenAI's GPT-3 language model. Developed by OthersideAI, Chronology offers an intuitive interface to streamline your workflow with GPT-3.

## Features

- **Asynchronous Calls**: Make multiple GPT-3 prompts asynchronously, enabling parallel processing.
- **Prompt Management**: Easily create, modify, and chain prompts.
- **Complex Systems**: Chain prompts together, using outputs from one or more prompts as inputs to others, facilitating rapid development of intricate systems.

## Installation

Chronology is available on PyPI and supports Python 3.6 and above.

To install Chronology, run:

```sh
pip install chronological
```

### Dependencies

Chronology requires the following packages:
- [`openai-api`](https://github.com/openai/openai-python)
- [`python-dotenv`](https://pypi.org/project/python-dotenv/)
- [`loguru`](https://github.com/Delgan/loguru)
- [`asyncio`](https://docs.python.org/3/library/asyncio.html)

The setup files require the following dependencies:
- [`colorama`](https://pypi.org/project/colorama/)

## Usage

After installing the package, create a `.env` file at the root of your project and add your OpenAI API key:

```env
OPENAI_API_KEY="YOUR_API_KEY"
```

### Using ChronologyUI

You can use [ChronologyUI](https://github.com/OthersideAI/chronology-ui) for generating chains. Here is a [Loom video](https://www.loom.com/share/47cb8d328ebd446db4d98ea1c0cac2c7?sharedAppSource=personal_library) demonstrating how to use the UI with the [`chronology`](https://github.com/OthersideAI/chronology) package.

### Using the API Directly

#### `main`

The `main` function is an async function that holds your business logic. Invoke this logic by passing it as an argument to `main`.

**Example:**

```python
# You can name this function anything you want; the name "logic" is arbitrary
async def logic():
    # Call Chronology functions, awaiting those that are marked await
    prompt = read_prompt('example_prompt')
    completion = await cleaned_completion(
        prompt,
        max_tokens=100,
        engine="davinci",
        temperature=0.5,
        top_p=1,
        frequency_penalty=0.2,
        stop=["\n\n"]
    )

    print(f'Completion Response: {completion}')

    # Run additional custom logic
    for i in range(4):
        print("hello")

# Invoke the Chronology main function to run the async logic
main(logic)
```

#### `fetch_max_search_doc`
**Must be awaited**

Fetch the document with the highest score using the OpenAI API Search.

**Options:**
- `min_score_cutoff`: If the highest score is less than this value, `None` will be returned. Defaults to -1.
- `full_doc`: Return the entire response with the highest score but does not grab the document for you. Defaults to `False`.

**Example:**

```python
result = await fetch_max_search_doc(query, docs, min_score_cutoff=0.5, full_doc=True)
```

#### `raw_completion`
**Must be awaited**

Wrapper for the OpenAI API completion, returning the raw result from GPT-3.

**Example:**

```python
result = await raw_completion(prompt, max_tokens=100)
```

#### `cleaned_completion`
**Must be awaited**

Wrapper for the OpenAI API completion, returning the whitespace-trimmed result from GPT-3.

**Example:**

```python
result = await cleaned_completion(prompt, max_tokens=100)
```

#### `gather`
**Must be awaited**

Run methods in parallel.

**Example:**

```python
results = await gather(
    fetch_max_search_doc(query1, docs),
    fetch_max_search_doc(query2, docs)
)
```

#### `read_prompt`

Read a text file from the `prompts/` directory. Pass only the file name, not the extension.

**Example:**

```python
prompt_text = read_prompt('hello-world')
```

#### `add_new_lines_start`

Add N new lines to the start of a string.

#### `add_new_lines_end`

Add N new lines to the end of a string.

#### `append_prompt`

Append new content to the end of a string.

#### `prepend_prompt`

Add new content to the start of a string.

#### `set_api_key`

Set your OpenAI API key within the code.

## Contributing

Chronology and ChronologyUI are open source and welcome contributions and feedback.

### Open Bounties

- Adding all fields accepted by the OpenAI Python API to Chronology
- Creating a test suite for different length chains
- Enhancing `fetch_max_search_doc` with smarter minimum score logic
- Optimizing `gather` to run faster using [threads](https://docs.python.org/3/library/asyncio-task.html#running-in-threads)

## Learn More

Chronology powers [OthersideAI](https://OthersideAI.com), helping us build complex applications with multiple steps efficiently. By parallelizing steps, Chronology significantly reduces the time required for tasks like email generation.

For more information about OthersideAI, check out:
- [Our Homepage](https://www.othersideai.com/)
- [Our Twitter](https://twitter.com/othersideai)

Contact us at: info@othersideai.com

# Quick Setup Script
```bash
pip install os subprocess platform colorama
```

Once those have been installed, you can paste the code below into VSCode or your IDE of choice. This will create the directory, install any packages, and is designed to be verbose.

```py
import os
import subprocess
import platform
from colorama import init, Fore, Style

init(autoreset=True)

# Define the project structure and files
project_name = "chronology_project"
env_file_content = 'OPENAI_API_KEY="YOUR_API_KEY"\n'
requirements = ["chronological", "openai", "python-dotenv", "loguru", "colorama"]

# Function to create directories and files
def create_project_structure():
    print(f"{Fore.BLUE}Creating project structure...{Style.RESET_ALL}")

    # Create project directory
    os.makedirs(project_name, exist_ok=True)

    # Create .env file
    with open(os.path.join(project_name, '.env'), 'w') as f:
        f.write(env_file_content)
    print(f"{Fore.GREEN}Created .env file.{Style.RESET_ALL}")

    # Create a simple main.py file
    main_py_content = '''
# --- Main.py File for Chronology ---
import os
from colorama import init, Fore, Style
init(autoreset=True)

def main():
    print(f"{Fore.GREEN}Project setup complete.{Style.RESET_ALL}")

if __name__ == "__main__":
    main()
'''
    with open(os.path.join(project_name, 'main.py'), 'w') as f:
        f.write(main_py_content)
    print(f"{Fore.GREEN}Created main.py file.{Style.RESET_ALL}")

# Function to install packages
def install_packages():
    print(f"{Fore.BLUE}Installing packages...{Style.RESET_ALL}")
    for package in requirements:
        subprocess.check_call([os.sys.executable, "-m", "pip", "install", package])
        print(f"{Fore.GREEN}Installed {package}.{Style.RESET_ALL}")

# Function to handle Windows-specific setup
def windows_setup():
    print(f"{Fore.BLUE}Running Windows setup...{Style.RESET_ALL}")
    create_project_structure()
    install_packages()

# Function to handle MacOS-specific setup
def macos_setup():
    print(f"{Fore.BLUE}Running MacOS setup...{Style.RESET_ALL}")
    create_project_structure()
    install_packages()

# Function to handle Linux-specific setup
def linux_setup():
    print(f"{Fore.BLUE}Running Linux setup...{Style.RESET_ALL}")
    create_project_structure()
    install_packages()

# Main script execution
if __name__ == "__main__":
    current_os = platform.system()
    if current_os == "Windows":
        windows_setup()
    elif current_os == "Darwin":
        macos_setup()
    elif current_os == "Linux":
        linux_setup()
    else:
        print(f"{Fore.RED}Unsupported OS: {current_os}{Style.RESET_ALL}")

    print(f"{Fore.GREEN}All set! Your project '{project_name}' has been created and is ready to use.{Style.RESET_ALL}")
```

**Sponsored**
Excellent AI [Tools](https://api.adzedek.com/click_toolify0314?chatbot_id=1715191360448x620213882279166000&operation_hash=3e3032bd73a3b6d9d9cef7fa954a9bcc) Directory. Discover the best AI websites & tools.

## Github Project Editing
- OthersideAI.com
- [Graham Waters](https://github.com/grahamwaters)


### Summary of Changes:
- Updated dependencies to include links for `colorama`.
- Refined explanations and added structure for better readability.
- Ensured all usage examples and installation instructions are clear and consistent.
