# What is this?
This is an example of how [Kustomize](https://github.com/kubernetes-sigs/kustomize) can be used for deploying the same K8S `.yaml` files to several environments (development and production)

# Repo Structure
```
├── base
│   ├── config.yaml
│   ├── jenkins-test.yaml
│   ├── kustomization.yaml
│   ├── pvc.yaml
│   ├── rbac.yaml
│   ├── secret.yaml
│   ├── service-account.yaml
│   ├── service.yaml
│   ├── statefullset.yaml
│   └── test-config.yaml
├── overlay
│   ├── development
│   │   ├── kustomization.yaml
│   │   ├── kustomizeconfig
│   │   │   └── prefix-transformer.yaml
│   │   └── service.yaml
│   └── production
│       ├── kustomization.yaml
│       ├── kustomizeconfig
│       │   └── prefix-transformer.yaml
│       └── service.yaml
└── README.md
```

base: Contain default manifest that generated from jenkins helm chart
overlay: Contain some manifest changes and modification for spesific environment

# How to render the yaml file from Helm?
The YAML file basically from [Jenkins-Charts](https://github.com/jenkinsci/helm-charts). In this scenario, we use [Jenkins-Charts](https://github.com/jenkinsci/helm-charts) with version 4.1.11.

To generate the yaml file, we can use `helm template` command, for this case, you can follow this steps:
1. clone the repository
```
git clone https://github.com/jenkinsci/helm-charts.git
cd helm-charts
```
2. checkout to the spesific version (ie. 4.1.11)
```
git checkout tags/jenkins-4.1.11
```
3. Run `helm template` command with spesific chart and values.yaml. in this example i use the spesific values.yaml and put in into manifest.yaml
```
helm template charts/jenkins/ -f <your-spesific-file-values.yaml> > <somewhere-path>/manifest.yaml
```

# How to generate manifest file from kustomize?
Just run this command `kustomize build <the-spesific-environment>`, for example:
```
kustomize build overlay/production
```