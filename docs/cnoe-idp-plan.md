
# CNOE IDP adoption plan

## Preparation

Necessary steps to customize a bundle of CNOE stacks for customer use-case.

1. Prepare custom stacks based on the stacks available at https://github.com/cnoe-io/stacks
  The reference-implementation stack should be part of that selection. It includes Backstage, ArgoCD, Argo Workflows, Keycloak for SSO.

2. Customizations should focus on backstage-templates/entities to create templates to satisfy client requirements.
  Their content is a combination of Backstage Template Entity with a skeleton of the resources that neeed to be created. The skeleton is the source for the GitHub repository that will be created and that is used by ArgoCD to manage resources using GitOps workflows.

  The custom entity templates should have the following layout

    ```sh
      ├── mkdocs.yml        # documentation
      ├── skeleton          # content of the GitOps repo for the new Entity created
      │   ├── catalog-info.yaml # source of Entity(ies) info that Backstage imports into the Catalog
      │   ├── docs              # sample docs layout
      │   │   ├── idpbuilder.md
      │   │   ├── images
      │   │   │   └── cnoe-logo.png
      │   │   └── index.md
      │   └── manifests         # in this entity case, it manages k8s manifests that are deployed to local k8s cluster.
      │       └── deployment.yaml
      └── template.yaml         # Backstage Template that glues all these resouces together
    ```
  
  See https://github.com/cnoe-io/stacks/tree/main/ref-implementation/backstage-templates/entities/basic

3. Replace Gitea with Github integration.

4. Define organizational units that are used by Backstage to create relationships among entities:
   Domain > Organization > Team > Members

   See https://backstage.io/docs/features/software-catalog/descriptor-format

## Deployment

1. Initial deployment with `idpbuilder create` command.
2. Subsequent updates and maintenance could be performed through ArgoCD.
   For example a new service could be deployed by creating a new ArgoCD application. 

## CNOE IDP Workflow

Templates use typical value substitution with `{{ }}` delimiters, and have a structure like pipelines where each step could invoke a built-in or a custom action. 

For example this template, https://github.com/cnoe-io/stacks/blob/main/ref-implementation/backstage-templates/entities/basic/template.yaml, has the following steps:

1. fetch:template to source the skeleton content.
2. publish:gitea to create new Gitea repository with the skeleton content
3. cnoe:create-argocd-app to create a new ArgoCD Application using the newly created Gitea repo as a source.
4. catalog:register to register the new app with Backstage Catalog.

The template has input parameters that could be passed to steps as values, and produce output that could be used by Backstage plugins for example to show a link to the newly created Entity in the Backstage Catalog.

Every custom template creatd will follow this pattern with maybe some extra steps. For example this more complex example creates entity that creates S3 bucket and performs interesting customization of kustomize overlays.

https://github.com/cnoe-io/stacks/blob/main/ref-implementation/backstage-templates/entities/app-with-bucket/template.yaml

Conceptually there are two distinct phases:

1. Backstage instantiates the new Entity and through it a new GitHub Repo and ArgoCD Application.
2. Management of the new resources linked to the Entity through GitOps workflow - GitHub Repo and ArgoCD.
   Backstage is automatically configured to track the Entity(ies) and its(their) resources

## Misc

The CNOE IDP is still in early stages of development. There were some bugs I had to fix to get the Templates working, but on the positive side this is a great opportunity to connect with the community, contribute to the project and get their help.
