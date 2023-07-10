# Code Review with OpenAI

This repository contains code that enables you to perform code reviews using OpenAI's language model. The code utilizes the OpenAI API, along with the GitHub Actions workflow, to automate the process of reviewing code changes in pull requests.

## Prerequisites

Before using this code, make sure you have the following:

- OpenAI API key: You need an API key to interact with OpenAI's language model. If you don't have one, sign up for OpenAI and obtain your API key.
- GitHub repository: You should have a GitHub repository where you want to enable code reviews. Ensure that you have the necessary permissions to add workflows and access pull requests.

## Setup

To enable code review with OpenAI on your GitHub repository, follow these steps:

1. Fork this repository: Click the "Fork" button at the top-right corner of this repository to create a copy of it in your own GitHub account.

2. Get the OpenAI API key: Sign up for OpenAI and obtain an API key that allows you to interact with OpenAI's language model.

3. Add the API key as a secret: In your forked repository, go to the "Settings" tab and click on "Secrets" in the left sidebar. Add a new secret called `OPENAI_API_KEY` and paste your OpenAI API key as the value.

4. Create a workflow file: In your forked repository, navigate to the `.github/workflows/` directory and create a new file called `code_review.yml`. Copy and paste the following YAML code into this file:

   ```yaml
   name: Code Review with OpenAI

   on:
     pull_request:

   jobs:
     review:
       runs-on: ubuntu-latest

       steps:
         - name: Checkout repository
           uses: actions/checkout@v2

         - name: Set up Python
           uses: actions/setup-python@v2
           with:
             python-version: 3.9

         - name: Install dependencies
           run: |
             pip install openai PyGithub GitPython

         - name: Run review
           env:
             OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
           run: python .github/actions/code_review.py
     ```

5. Commit and push the changes: Commit the new workflow file (`code_review.yml`) to your forked repository and push the changes to GitHub. That's it! The code review workflow is now set up on your GitHub repository. Whenever a pull request is opened or updated, the workflow will automatically execute and post the code review as a comment on the pull request.

Note: Make sure to keep your OpenAI API key and GitHub token confidential. Avoid sharing them publicly or committing them to a public repository.

## Usage

To use the code review functionality, simply follow the setup instructions provided above. The workflow will automatically trigger on every pull request event, clone the repository, install the dependencies, and run the `code_review.py` script. The code review generated by OpenAI will be posted as a comment on the pull request.

## Customization

You can customize the behavior of the code review process by modifying the `code_review.py` file. Here are a few possible modifications:

- Adjusting the token limit: The `TOKEN_LIMIT` variable sets the maximum token limit for the language model. You can modify this value according to your requirements.

- Modifying the review prompt: The initial user message to OpenAI can be customized by modifying the `content` field in the `messages` list.

- Adding error handling: You can enhance the error handling logic in the code to handle specific scenarios or exceptions more effectively.

- Extending the review process: If you want to perform additional actions based on the review, you can modify the `post_comment` function or add new functions as needed.

## Warning

While leveraging the power of the ChatGPT API and external services like OpenAI can be incredibly beneficial, it’s crucial to exercise caution when dealing with sensitive code. The ChatGPT model processes data externally, which means the code you submit for review is shared with the OpenAI infrastructure. It’s essential to ensure that the code being reviewed does not contain any sensitive information that you are not comfortable sharing with an external company.

## Conclusion

By following the setup instructions above, you can enable code reviews using OpenAI's language model directly on your GitHub repository. The system will generate reviews for pull requests, helping to identify problematic code, highlight potential issues, and evaluate the overall quality of the code. Customize the code and workflow to suit your specific needs and improve your code review process.
