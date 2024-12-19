
## Infrastructure Code Workflow/Pipeline

When we make any change to the staging branch, the workflow will detect that and the Terraform code will be tested.Basically, it will run `terraform validate` and `terraform plan` to test or check our code against AWS Cloud and what are the differences that it's going to apply. It won't apply in this staging phase but will test and show the results.

So let's say you make a change in staging and you like it. At this point you would make a pull request to merge the staging branch with the main branch. Once the pull request is approved, this approval will mean a merge will happen between staging and main branch. When the main branch receives this merge/new change, it will be detected by the workflow and the same code, which is now in the main branch will be applied to the infrastructure. For us right now that means AWS (vpc, eks ). So when applied for the first time, it will create the EKS cluster. And whenver you make any change to the Terraform code, the staging branch will be tested, pull request will be created and it will be approved and then the main branch will be merged with the staging branch, and the changes will be applied to the infrastructure.

We can call this a workflow or pipeline.FYI~ In Github it is called workflow Means same.

## App Code Workflow/pipeline

We also have the App Code, docker file, and Kubernetes definition files. This is for app changes. We'll have a workflow whcih wil fetch the code, build the code, test the code and deploy the code. In this probject we'll have Maven, Check Style and Sonar Code Analysis which will evalaute it using its sonar quality gates. If everything checks out fine, then it's going to build docker images and upload it to Amazon ECR IOW: the docker registry.

We will have Helm Charts which will bundle our Kubernetes definition files. It will also have a variable whcih will have a tag name for the image, the image name and tag name. Basically where to fetch the image and what version of the image to fetch. We will pass this in the workflow automatically. This will be the same tag we use to build the docker image will be passed to the Helm Charts and the Helm Charts gets executed on our EKS cluster. Now here the Kubernetes cluster is going to detect the change of the image tag. It's goign to fetch the image from ECR and run your application.

Note we had to fork both repos from Udemys instructors repo. We of course need ssh kys to push

cd `Gitops`

`git clone git@github.com:lazyjitsu/iac-vprofile.git`
`git clone git@github.com:lazyjitsu/vprofile-action.git`

We also need to make sure that when we use the repo, it uses the ssh ky we dictate, the one we created.
`cd iac-vprofile`

_note we are referring to our private ky here..hence no _.pub at the end of the file\*

`git config core.sshCommand "ssh -i ~/.ssh/$myky -F /dev/null"

If you haven't set the user and email:
LENOVO USER@GraphMach MINGW64 /d/mySource/Training/DevOps/Udemy/DevOps Beginners to Advanced with Projects/GitOps (master)
`git config --global user.name devops4sure`
`git config --global user.email devops4sure@gmail.com`
`cd iac-vprofile`
`git branch -a`
`git checkout stage`

```
LENOVO USER@GraphMach MINGW64 /d/mySource/Training/DevOps/Udemy/DevOps Beginners to Advanced with Projects/GitOps/iac-vprofile (stage)
$ gl
92b9d9a (HEAD -> stage, origin/stage) Read
97a2dda br p
191764a Freashly baked
d89b3ed Freashly baked
b878de8 Freashly baked
bc57c11 Freashly baked
```

Udemy says to stay in staging branch. We are going to make changes to staging branch znd if it tests fine, we'll merge it in the main branch. When merged in the main branch, that' when it will apply the Terraform changes.

# Github g

In this lesson we are giong to create AWS access kys, an S3 bucket and ECR repository on AWS Cloud. We will store all this information in Github g.

Create a user, we called it 'gitops' and generate access ky after giving gitops full administrative rights for expeditious sake. We would assign a more thoughtful policy(ies) when in real world.

Goto github > your repo (iac-vprofile) in this case > settings > secrts & variables > New repository sect::
Name[AWS_ACCESS_Ky_ID]
Secrt[$val]

 New Repository sect::
 Name[AWS_SECT_Ky]

Goto githb you repo (the other repo:vprofile-action )
Name[AWS_ACCESS_Ky_ID]

 Add sect > New repository sect:

Name[AWS_SECT_Ky]

 Add sect > New Repository sect

Dont forget to click `done` on your `gitops` user add in IAM. We momemtarily left that after creating the access id/ sect thing.

Now we need to create the S3 Bucket.
Because it needed to be unique `vprofileactions1414` is the name of our bucket. Copy the bucket and back to repo's g. So this sect ONLY goes in `iac-vprofile` where we have the terraform code

Now goto ECR (Elastic Container Registry) > Create repository::
Repository name[vprofileapp]

 `create`

Copy the url, in this case,

`882376726113.dkr.ecr.us-east-1.amazonaws.com/vprofileappimg`

Goto g of the vprofile-action repo. Create a sect for the ECR like so:
Name [REGISTRY]
Remove the vprofileapp at the end. just the url
sect [882376726113.dkr.ecr.us-east-1.amazonaws.com]

 `Add sect`
