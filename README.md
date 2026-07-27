name: First Workflow -->Name of the workflow
on: workflow_dispatch -.>manual action when u add this it will show a button to run the workflow
jobs: --->jobs
  first-job: -->name of the job
    runs-on: ubuntu-latest-->on which runner it ha sto run
    steps: -->what it has to do
      - name: Print greeting
        run: echo "Hello World!" -->it is just printing
      - name: Print goodbye
        run: echo "Done - bye!"


Running Multi-Line Shell Commands
Thus far, you learned how to run simple shell commands like echo "Something" via run: echo "Something".

If you need to run multiple shell commands (or multi-line commands, e.g., for readability), you can easily do so by adding the pipe symbol (|) as a value after the run: key.

Like this:

...
run: |
    echo "First output"
    echo "Second output"
This will run both commands in one step.


To use actions
uses: <action nane>
suppose checkout action


When the workflow runs, actions/checkout@v3:
• Clones the repository into the runner workspace.
• Checks out the commit/branch that triggered the workflow.
• Makes the code available for build, test, lint, and deployment steps.

Why it's needed
Without actions/checkout, the runner generally does not have your repository files available, so commands like:
ls
npm install
mvn test
dotnet build

would not find your project source code

