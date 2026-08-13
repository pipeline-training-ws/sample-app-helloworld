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

The Jenkinsfile in the CI template folder loads this config at runtime:

```groovy
Map configMap = readYaml 'ci-config.yaml'
pipelineMaven(configMap)
```

The `env.SHAREDLIB_GIT_TAG` (and related vars) should be injected at **Folder** or **Controller** level
so the correct version of the shared library is loaded without modifying the Jenkinsfile.

---

## Related Resources

- [`ci-shared-library/vars/pipelineTemplateHelloWorld.groovy`](../ci-shared-library/vars/pipelineTemplateHelloWorld.groovy)
- [`ci-templates/templates/0-helloWorld/Jenkinsfile`](../ci-templates/templates/0-helloWorld/Jenkinsfile)
- [`ci-templates/templates/1-helloWorldSharedLib/Jenkinsfile`](../ci-templates/templates/1-helloWorldSharedLib/Jenkinsfile)
- [CloudBees CI Shared Libraries documentation](https://docs.cloudbees.com/docs/cloudbees-ci/latest/pipelines/shared-libraries)
