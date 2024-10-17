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



