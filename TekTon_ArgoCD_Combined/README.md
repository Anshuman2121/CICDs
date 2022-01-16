# Tekton and Argo CD PoC

This is a PoC to check Tekton, Argo CD and how both tools can work together following a GitOps way

With this post I’m going to show how a modern and cloud native CI/CD could be implemented within a Kubernetes environment. I’ll use two different tools:

- Tekton: to implement CI stages
- Argo CD: to implement CD stages (Gitops)

![Alt text](resources/pipeline-stages.png?raw=true "Stages")


In this pipeline, we can see two different parts:

* CI part, implemented by Tekton and ending with a stage in which a push to a repository is done.
* Checkout: in this stage, source code repository is cloned
* Build & Test: in this stage, we use Maven to build and execute test
* Code Analisys: code is evaluated by Sonarqube
* Publish: if everything is ok, artifact is published to Nexus
* Build image: in this stage, we build the image and publish to local registry
* Push to GitOps repo: this is the final CI stage, in which Kubernetes descriptors are cloned from the GitOps repository, they are modified in order to insert commit info and then, a push action is performed to upload changes to GitOps repository
* CD part, implemented by Argo CD, in which Argo CD detects that the repository has changed and perform the sync action against the Kubernetes cluster.
* Does it mean that we can not implement the whole process using Tekton? No, it doesn’t. It’s possible to implement the whole process using Tekton but in this case, I want to show the Gitops concept.

mv poc/conf/argocd/git-repository.yaml.bk poc/conf/argocd/git-repository.yaml
mv poc/conf/tekton/git-access/secret.yaml.bk poc/conf/tekton/git-access/secret.yaml

Add Github token
- poc/conf/argocd/git-repository.yaml
- poc/conf/tekton/git-access/secret.yaml


To Install
make install

To Uninstall
make uninstall

