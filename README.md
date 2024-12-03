#   #Jenkins


### Code Checkout for scripted pipeline
```groovy

    stage('Code Checkout') {
      
      sh 'find . | grep "^./" |xargs rm -rf'

      if (env.TAG_NAME ==~ ".*") {
        env.gitbrname = "refs/tags/${env.TAG_NAME}"
      } else {
        env.gitbrname = "${env.BRANCH_NAME}"
      }
      checkout poll: false, scm: [
          $class: 'GitSCM', 
          userRemoteConfigs: [
              [
                  url: "https://github.com/raghudevopsb80/${env.appName}"
              ]
          ], 
          branches: [
              [
                  name: gitbrname
              ]
          ]
      ]

    }

```


# Argocd #ArgoCD Helm Chart

```terraform
resource "helm_release" "argocd" {
  depends_on = [
    null_resource.kube-config,
    helm_release.nginx-ingress
  ]

  name             = "argocd"
  repository       = "https://argoproj.github.io/argo-helm"
  chart            = "argo-cd"
  namespace        = "argocd"
  create_namespace = true

  set {
    name  = "global.domain"
    value = "argocd-${var.name}-${var.env}.rdevopsb79.online"
  }

  values = [
    file("${path.module}/conf/argocd.yaml")
  ]

}
```

### Get ArgoCD admin password 
`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`



## Cloudwatch KMS Access

```json
{
    "Version": "2012-10-17",
    "Id": "key-consolepolicy-3",
    "Statement": [
        {
            "Sid": "Enable IAM User Permissions",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::633788536644:root"
            },
            "Action": "kms:*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "logs.us-east-1.amazonaws.com"
            },
            "Action": [
                "kms:Encrypt*",
                "kms:Decrypt*",
                "kms:ReEncrypt*",
                "kms:GenerateDataKey*",
                "kms:Describe*"
            ],
            "Resource": "*",
            "Condition": {
                "ArnEquals": {
                    "kms:EncryptionContext:aws:logs:arn": "arn:aws:logs:us-east-1:633788536644:log-group:*"
                }
            }
        }
    ]
}
```
Ref link: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html

###  Ansible Certification

https://entersoftlabs.com/certification/red-hat-ansible-automation-ex407-certification-in-hyderabad

### Pipeline XML

```xml
<?xml version='1.1' encoding='UTF-8'?>
<flow-definition plugin="workflow-job@1436.vfa_244484591f">
  <actions/>
  <description></description>
  <keepDependencies>false</keepDependencies>
  <properties>
    <hudson.model.ParametersDefinitionProperty>
      <parameterDefinitions>
        <hudson.model.StringParameterDefinition>
          <name>TEST</name>
          <description>TEST</description>
          <trim>false</trim>
        </hudson.model.StringParameterDefinition>
      </parameterDefinitions>
    </hudson.model.ParametersDefinitionProperty>
  </properties>
  <definition class="org.jenkinsci.plugins.workflow.cps.CpsScmFlowDefinition" plugin="workflow-cps@3964.v0767b_4b_a_0b_fa_">
    <scm class="hudson.plugins.git.GitSCM" plugin="git@5.5.1">
      <configVersion>2</configVersion>
      <userRemoteConfigs>
        <hudson.plugins.git.UserRemoteConfig>
          <url>https://github.com</url>
          <credentialsId>admin</credentialsId>
        </hudson.plugins.git.UserRemoteConfig>
      </userRemoteConfigs>
      <branches>
        <hudson.plugins.git.BranchSpec>
          <name>*/master</name>
        </hudson.plugins.git.BranchSpec>
      </branches>
      <doGenerateSubmoduleConfigurations>false</doGenerateSubmoduleConfigurations>
      <submoduleCfg class="empty-list"/>
      <extensions/>
    </scm>
    <scriptPath>Jenkinsfile</scriptPath>
    <lightweight>true</lightweight>
  </definition>
  <triggers/>
  <disabled>false</disabled>
</flow-definition>
```


#   #Jenkins


### Code Checkout for scripted pipeline
```groovy

    stage('Code Checkout') {
      
      sh 'find . | grep "^./" |xargs rm -rf'

      if (env.TAG_NAME ==~ ".*") {
        env.gitbrname = "refs/tags/${env.TAG_NAME}"
      } else {
        env.gitbrname = "${env.BRANCH_NAME}"
      }
      checkout poll: false, scm: [
          $class: 'GitSCM', 
          userRemoteConfigs: [
              [
                  url: "https://github.com/raghudevopsb80/${env.appName}"
              ]
          ], 
          branches: [
              [
                  name: gitbrname
              ]
          ]
      ]

    }

```


# Argocd #ArgoCD Helm Chart

```terraform
resource "helm_release" "argocd" {
  depends_on = [
    null_resource.kube-config,
    helm_release.nginx-ingress
  ]

  name             = "argocd"
  repository       = "https://argoproj.github.io/argo-helm"
  chart            = "argo-cd"
  namespace        = "argocd"
  create_namespace = true

  set {
    name  = "global.domain"
    value = "argocd-${var.name}-${var.env}.rdevopsb79.online"
  }

  values = [
    file("${path.module}/conf/argocd.yaml")
  ]

}
```

### Get ArgoCD admin password
`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`



## Cloudwatch KMS Access

```json
{
    "Version": "2012-10-17",
    "Id": "key-consolepolicy-3",
    "Statement": [
        {
            "Sid": "Enable IAM User Permissions",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::633788536644:root"
            },
            "Action": "kms:*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "logs.us-east-1.amazonaws.com"
            },
            "Action": [
                "kms:Encrypt*",
                "kms:Decrypt*",
                "kms:ReEncrypt*",
                "kms:GenerateDataKey*",
                "kms:Describe*"
            ],
            "Resource": "*",
            "Condition": {
                "ArnEquals": {
                    "kms:EncryptionContext:aws:logs:arn": "arn:aws:logs:us-east-1:633788536644:log-group:*"
                }
            }
        }
    ]
}
```
Ref link: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html

###  Ansible Certification

https://entersoftlabs.com/certification/red-hat-ansible-automation-ex407-certification-in-hyderabad

### gh cli install for GitHub Runner.

```shell
# Following steps need to be run manually
# sudo curl -L -o /etc/yum.repos.d/gh-cli.repo https://cli.github.com/packages/rpm/gh-cli.repo
# sudo dnf install gh -y
# gh auth login -s admin:org

```


### Patch service from ClusterIP to Loadblancer  and get admin secret
```shell
kubectl patch svc argocd-server -p '{"spec": {"ports": [{"port": 443,"targetPort": 8080,"name": "https"},{"port": 80,"targetPort": 8080,"name": "http"}],"type": "LoadBalancer"}}' -n argocd
kubectl get secrets argocd-initial-admin-secret -n argocd --template={{.data.password}} | base64 --decode
```

### AZ Terraform 
```bash 
az ad sp create-for-rbac -n terraform --role Contributor --scopes /subscriptions/7b6c642c-6e46-418f-b715-e01b2f871413
```





