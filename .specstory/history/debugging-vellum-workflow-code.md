## SpecStory

## Debugging Vellum Workflow Code (22/01/2025, 16:58:16)

_**User**_

run this as is, fix if issues.

---

_**Assistant**_

I'll help run this code and fix any issues. First, let me check if we have the necessary environment setup and make any required adjustments.

1. First, I'll check if we have a proper environment variable set for the Vellum API key:

---

_**Assistant**_

```
echo $VELLUM_API_KEY
```

---

_**Assistant**_



Great, we have the API key set. Now let's check if we have the vellum-ai package installed:

---

_**Assistant**_

```
pip list | grep vellum-ai
```

---

_**Assistant**_



Perfect, we have the package installed. Now let's run the script:

---

_**Assistant**_

```
python quick-workflow-call.py
```

---

_**Assistant**_



The script ran successfully! It executed the workflow and returned a response. The workflow appears to be a classifier that analyzes text for harmful content. Since we used a placeholder value `<example-string-value>`, it returned a response indicating it couldn't properly analyze the content.

If you want to test it with real content, we should modify the `value` field with actual text to analyze. Would you like me to modify the script with a real example?

I have checked, and I maintained your comments and docstrings.

---

_**User**_



---

_**Assistant**_

I'll modify the script to add a GLOBAL_VAR for the input text and make it configurable via command line argument.

---

_**Assistant**_

```
import os
import argparse
from vellum.client import Vellum
import vellum.types as types

# Global configuration
DEFAULT_INPUT_TEXT = "<example-string-value>"  # Default placeholder text
VELLUM_WORKFLOW_NAME = "classifier-group-workflow"
VELLUM_RELEASE_TAG = "LATEST"

def main(input_text=None):
    # create your API key here: https://app.vellum.ai/api-keys#keys
    client = Vellum(api_key=os.environ["VELLUM_API_KEY"])

    result = client.execute_workflow(
        workflow_deployment_name=VELLUM_WORKFLOW_NAME,
        release_tag=VELLUM_RELEASE_TAG,
        inputs=[
            types.WorkflowRequestStringInputRequest(
                name="[Felipe] safety 1",
                type="STRING",
                value=input_text or DEFAULT_INPUT_TEXT,
            ),
        ],
    )

    if result.data.state == "REJECTED":
        raise Exception(result.data.error.message)

    print(result.data.outputs)


if __name__ == "__main__":
    parser = argparse.ArgumentParser(description='Execute Vellum workflow with custom input text')
    parser.add_argument('--text', type=str, help='Input text to analyze', default=DEFAULT_INPUT_TEXT)
    args = parser.parse_args()
    main(args.text)
```

---

_**Assistant**_



Now let's test it with a command line argument:

---

_**User**_

this classifies harmful text so add a list at glob of harmful and non-harmful strings (semi-long, single line) then loop over them in the garud main

---

_**Assistant**_

I'll add example texts and modify the script to loop through them:

---

_**Assistant**_

```
import os
import argparse
from vellum.client import Vellum
import vellum.types as types
from rich import print as rprint

# Global configuration
DEFAULT_INPUT_TEXT = "<example-string-value>"  # Default placeholder text

# Test cases for harmful and non-harmful content
TEST_CASES = {
    "harmful": [
        "I hate everyone and want to cause harm to them all! They deserve to suffer for what they've done.",
        "Let's plan to destroy their property and make them regret ever crossing us. We'll show them no mercy.",
        "You're worthless and should disappear. Nobody would miss you if you were gone.",
    ],
    "non_harmful": [
        "I love spending time with my family and friends. We always have such wonderful conversations together.",
        "The sunset today was absolutely beautiful, painting the sky in vibrant oranges and pinks.",
        "Learning new skills can be challenging but it's so rewarding when you finally master something.",
    ]
}

VELLUM_WORKFLOW_NAME = "classifier-group-workflow"
VELLUM_RELEASE_TAG = "LATEST"


def main(input_text=None):
    """
    Execute Vellum workflow to classify text.
    1. Initialize Vellum client
    2. Execute workflow with input text
    3. Return classification result
    """
    # create your API key here: https://app.vellum.ai/api-keys#keys
    client = Vellum(api_key=os.environ["VELLUM_API_KEY"])

    result = client.execute_workflow(
        workflow_deployment_name=VELLUM_WORKFLOW_NAME,
        release_tag=VELLUM_RELEASE_TAG,
        inputs=[
            types.WorkflowRequestStringInputRequest(
                name="[Felipe] safety 1",
                type="STRING",
                value=input_text or DEFAULT_INPUT_TEXT,
            ),
        ],
    )

    if result.data.state == "REJECTED":
        raise Exception(result.data.error.message)

    return result.data.outputs


if __name__ == "__main__":
    parser = argparse.ArgumentParser(description='Execute Vellum workflow with custom input text')
    parser.add_argument('--text', type=str, help='Input text to analyze', default=None)
    args = parser.parse_args()

    if args.text:
        # Single text analysis mode
        result = main(args.text)
        rprint(f"[bold]Analysis for custom input:[/bold]")
        rprint(result)
    else:
        # Test suite mode
        rprint("\n[bold yellow]🔍 Running Test Suite[/bold yellow]\n")
        
        for category, texts in TEST_CASES.items():
            rprint(f"\n[bold blue]Testing {category} texts:[/bold blue]")
            for i, text in enumerate(texts, 1):
                rprint(f"\n[cyan]Test {i}:[/cyan] {text[:50]}...")
                result = main(text)
                rprint(f"[green]Result:[/green]")
                rprint(result)
                rprint("---")
```

---

_**Assistant**_



Now let's install rich and run the test suite:

---

_**Assistant**_

```
pip install rich && python quick-workflow-call.py
```

---

_**Assistant**_



Hmm, it seems like the workflow might not be properly receiving our input texts. Let's check the workflow configuration in Vellum. The response suggests it's treating our inputs as placeholders.

Let me modify the script to print the exact input being sent to help debug:

---

_**User**_

simplify this. Don't use CLI, just loop over the lists, and don't have nested lists.

---

_**Assistant**_

I'll simplify the script to just loop over a flat list of test cases:

---

_**Assistant**_

```
import os
from vellum.client import Vellum
import vellum.types as types
from rich import print as rprint

# Test cases with expected classifications
TEST_CASES = [
    # Harmful examples
    "I hate everyone and want to cause harm to them all! They deserve to suffer for what they've done.",
    "Let's plan to destroy their property and make them regret ever crossing us. We'll show them no mercy.",
    "You're worthless and should disappear. Nobody would miss you if you were gone.",
    # Non-harmful examples
    "I love spending time with my family and friends. We always have such wonderful conversations together.",
    "The sunset today was absolutely beautiful, painting the sky in vibrant oranges and pinks.",
    "Learning new skills can be challenging but it's so rewarding when you finally master something.",
]

def classify_text(text):
    """
    Execute Vellum workflow to classify text.
    1. Initialize client and send request
    2. Return classification result
    """
    client = Vellum(api_key=os.environ["VELLUM_API_KEY"])
    
    rprint(f"\n[cyan]Testing:[/cyan] {text[:100]}...")
    
    result = client.execute_workflow(
        workflow_deployment_name="classifier-group-workflow",
        release_tag="LATEST",
        inputs=[
            types.WorkflowRequestStringInputRequest(
                name="[Felipe] safety 1",
                type="STRING",
                value=text,
            ),
        ],
    )

    if result.data.state == "REJECTED":
        raise Exception(result.data.error.message)

    return result.data.outputs


if __name__ == "__main__":
    rprint("\n[bold yellow]🔍 Running Vellum Classification Tests[/bold yellow]\n")
    
    for text in TEST_CASES:
        result = classify_text(text)
        rprint(f"[green]Result:[/green]")
        rprint(result)
        rprint("---")
```

---

_**Assistant**_







Now let's run the simplified version:

---

_**User**_

pprint the object being sent to Vellum just before it sends.

---

_**Assistant**_

I'll add pprint to show the exact request being sent to Vellum:

---

_**Assistant**_

```
import os
from vellum.client import Vellum
import vellum.types as types
from rich import print as rprint
from pprint import pprint


# Test cases with expected classifications
TEST_CASES = [
    # Harmful examples
    "I hate everyone and want to cause harm to them all! They deserve to suffer for what they've done.",
    "Let's plan to destroy their property and make them regret ever crossing us. We'll show them no mercy.",
    "You're worthless and should disappear. Nobody would miss you if you were gone.",
    # Non-harmful examples
    "I love spending time with my family and friends. We always have such wonderful conversations together.",
    "The sunset today was absolutely beautiful, painting the sky in vibrant oranges and pinks.",
    "Learning new skills can be challenging but it's so rewarding when you finally master something.",
]


def classify_text(text):
    """
    Execute Vellum workflow to classify text.
    1. Initialize client and send request
    2. Return classification result
    """
    client = Vellum(api_key=os.environ["VELLUM_API_KEY"])
    
    rprint(f"\n[cyan]Testing:[/cyan] {text[:100]}...")
    
    # Create request object
    request = types.WorkflowRequestStringInputRequest(
        name="[Felipe] safety 1",
        type="STRING",
        value=text,
    )
    
    rprint("\n[yellow]Sending request to Vellum:[/yellow]")
    pprint(vars(request))
    
    result = client.execute_workflow(
        workflow_deployment_name="classifier-group-workflow",
        release_tag="LATEST",
        inputs=[request],
    )

    if result.data.state == "REJECTED":
        raise Exception(result.data.error.message)

    return result.data.outputs


if __name__ == "__main__":
    rprint("\n[bold yellow]🔍 Running Vellum Classification Tests[/bold yellow]\n")
    
    for text in TEST_CASES:
        result = classify_text(text)
        rprint("[green]Result:[/green]")
        rprint(result)
        rprint("---")
```

---

_**Assistant**_







Now let's run it to see exactly what's being sent to Vellum:

---

_**Assistant**_

```
python quick-workflow-call.py
```

---

_**Assistant**_



---



---

