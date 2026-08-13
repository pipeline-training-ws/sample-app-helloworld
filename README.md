# sample-app-helloWorld

A minimal sample application used as a **training reference** for the CloudBees CI / Jenkins Pipeline template labs.
It contains no production code — its sole purpose is to demonstrate how an application repository is structured
when adopting the **Pipeline-as-Code** pattern with a centralised Shared Library.

---

## Purpose

| Goal | Detail |
|------|--------|
| Demonstrate `ci-config.yaml` | Shows how per-application CI configuration is declared and consumed by the shared pipeline template |
| Shared Library integration | Works with `pipelineTemplateHelloWorld` from `ci-shared-library` |
| Lab exercises | Used in the *Hello World* lab series (templates `0-helloWorld` and `1-helloWorldSharedLib`) |

---

## Repository Structure

```
sample-app-helloWorld/
├── Jenkinsfile      # Imports ci-shared-library and invokes pipelineTemplateHelloWorld
├── ci-config.yaml   # Application-level CI configuration consumed by the pipeline template
└── README.md        # This file
```

---

## ci-config.yaml Reference

```yaml
app: 'Hello World'        # Human-readable application name
ci:
  hello: world            # Example custom CI parameter passed to the shared library
  firstName: Andreas      # Passed to helloWorld shared library step
  lastName: Caternberg    # Passed to helloWorld shared library step
```

All keys under `ci:` are forwarded to the pipeline template as a `Map config` and are accessible
inside Groovy shared library steps via `config.ci.<key>`.

---

## How It Is Used

The `Jenkinsfile` in this repository dynamically imports `ci-shared-library`, loads `ci-config.yaml`,
and invokes the `pipelineTemplateHelloWorld` step:

```groovy
library identifier: "${env.SHAREDLIB_GIT_REPO}@${env.SHAREDLIB_GIT_TAG_}", retriever: modernSCM(
        [$class: 'GitSCMSource',
         remote: "${env.SHAREDLIB_GIT_SERVER}/${env.SHAREDLIB_GIT_ORG}/${env.SHAREDLIB_GIT_REPO}.git",
         credentialsId: "${env.SHAREDLIB_GIT_CREDENTIALS}"
        ]
)

Map configMap = readYaml file: 'ci-config.yaml'
pipelineTemplateHelloWorld(configMap)
```

The `env.SHAREDLIB_GIT_SERVER`, `env.SHAREDLIB_GIT_ORG`, `env.SHAREDLIB_GIT_REPO`, `env.SHAREDLIB_GIT_TAG`,
and `env.SHAREDLIB_GIT_CREDENTIALS` vars each default to a sensible value in the `Jenkinsfile` but can be
overridden at **Folder** or **Controller** level so the correct version/location of the shared library is
loaded without modifying the `Jenkinsfile`.

> **Note:** `pipelineTemplateHelloWorld` currently reads `config.hello`, `config.firstName`, and
> `config.lastName` directly off the map passed to it, while `ci-config.yaml` nests these values under
> a `ci:` key. This mismatch also exists in the `1-helloWorldSharedLib` reference template and means
> those values resolve to `null` at runtime until the library step or the config structure is aligned.

---

## Related Resources

- [`ci-shared-library/vars/pipelineTemplateHelloWorld.groovy`](../ci-shared-library/vars/pipelineTemplateHelloWorld.groovy)
- [`ci-templates/templates/0-helloWorld/Jenkinsfile`](../ci-templates/templates/0-helloWorld/Jenkinsfile)
- [`ci-templates/templates/1-helloWorldSharedLib/Jenkinsfile`](../ci-templates/templates/1-helloWorldSharedLib/Jenkinsfile)
- [CloudBees CI Shared Libraries documentation](https://docs.cloudbees.com/docs/cloudbees-ci/latest/pipelines/shared-libraries)
