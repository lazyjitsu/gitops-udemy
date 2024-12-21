# Writing Our Workflow (i.e. Pipeline)
- Write Code in our own repository. This means github for us
- Create the directory structure
    - mkdir .github/workflows
- Any file with the extension .yaml/.yml that exists in the workflows folder, is how we create the workflow on github repo



$ pwd
/d/mySource/Training/DevOps/Udemy/DevOps Beginners to Advanced with Projects/GitOps/iac-vprofile
$ tree
Folder PATH listing for volume LENOVO
Volume serial number is 0CBC-E19B
D:.
├───.github
│   └───workflows
└───terraform


touch .github/workflows/terraform.yml

IMU that the fact that you have any yaml file ni the workflows is a trigger to github to run but when you define on and push, github workflows will stop and check to get its orders on what to do! An excerpt of this file for a reminder
## terraform.yml
name: "Vprofile IAC"
on: 
    push:
        branches:
            # defined in list format
            - main
            - stage
        paths:
            # Any file in the terraform folder. See tree output above
            - terraform/**
    pull_request:
        branches:
            - main
        paths:
            - terraform/**
    env:
        # Creds for AWS deployment
         AWS_ACCESS_KEY_ID: ${{secrets.AWS_ACCESS_KEY_ID}}
         AWS_SECRET_ACCESS_KEY_ID: ${{secrets.AWS_SECRET_ACCESS_KEY_ID}}

