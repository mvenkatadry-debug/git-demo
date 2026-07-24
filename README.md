name: First Workflow -->Name of the workflow
on: workflow_dispatch -.>manual action
jobs: --->jobs
  first-job: -->name of the job
    runs-on: ubuntu-latest-->on which runner it ha sto run
    steps: -->what it has to do
      - name: Print greeting
        run: echo "Hello World!" -->it is just printing
      - name: Print goodbye
        run: echo "Done - bye!"
