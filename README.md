# GPT-PenetrationTesting
GPT-PenetrationTesting is an innovative assistant for penetration testing, we used the OpenHermes-2.5-Mistral-7B model, we jailbroke it, finetuned it with commands for popular Kali Linux tools and it's now able to provide guided, actionable steps and command automation for performing deep pen tests.

## Setup and Installation

### Requirements

- Operating System: Kali Linux, Windows, or MacOS
- Python version 3.x
- Internet access for downloading necessary files and tools

### Installation Steps

1. **Install Python Libraries**: Use `pip install transformers colorama torch` to install required libraries.
2. **Download the Model File**: Obtain `https://github.com/pwnlaboratory/GPT-PenetrationTesting/` and note its path.
3. **Update Script**: Modify the `model_path` variable in the script to the model file's location.

### Execution

Run the script using `python pentest_ai.py` and follow the interactive prompts.

## Code Explanation with Snippets

### Check and Install Tools

The script starts by checking if necessary penetration testing tools are installed, installing any missing ones:

```python
def check_and_install_tools(tools):
    for tool in tools:
        result = subprocess.run(['which', tool], stdout=subprocess.PIPE)
        if not result.stdout.strip():
            subprocess.run(['sudo', 'apt-get', 'install', '-y', tool])
```

This function checks each tool in the `tools` list, installing it using `apt-get` if not found.

### Model Initialization

The script loads the `OpenHermes-2.5-Mistral-7B` model with a specific path and configuration:

```python
tokenizer = AutoTokenizer.from_pretrained("pwnlaboratory/GPT-PenetrationTesting")
model_path = "<path_to_model>"
model = AutoModelForCausalLM.from_pretrained(model_path, gpu_layers=12, threads=1)
```

This initializes the tokenizer and model, setting parameters like `gpu_layers` and `threads` for performance optimization.

### Interactive User Interface

The script interacts with the user, asking for input and providing guidance based on the user's actions:

```python
sys_env = input("Select your environment (1, 2, or 3): ")
if sys_env == '1':
    check_and_install_tools(pentest_tools)
```

It prompts the user to select their operating system environment and, if Kali Linux is chosen, it checks for and installs the necessary tools.

### Command Execution

For Kali Linux, the script can execute pentesting commands automatically:

```python
def execute_tool_command(output, ip_address):
    if 'nmap' in output:
        command_output = os.popen(f'nmap -sV {ip_address}').read()
        print(command_output)
```

This function parses the assistant's output for tool names and executes the corresponding command, showing the command output to the user.

## Conclusion

GPT-PenetrationTesting is designed to streamline the penetration testing process by integrating AI-powered guidance with practical command execution, making it a powerful tool for security professionals and enthusiasts alike.
