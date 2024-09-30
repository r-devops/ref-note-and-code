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

  set {
    name  = "server.ingress.enabled"
    value = true
  }

  set {
    name  = "server.ingress.ingressClassName"
    value = "nginx"
  }

  values = [
    file("${path.module}/conf/argocd.yaml")
  ]

}
```