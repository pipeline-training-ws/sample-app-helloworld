/**
Here we see how the entire pipeline gets externalized as a template to a Shared Library
env.SHARED_ vars should be injected from (Folder) environment properties
Alternative, the Shared Library can be coconfigured either on folder level or Controller level
**/

env.SHAREDLIB_GIT_SERVER = env.SHAREDLIB_GIT_SERVER ?: "https://github.com"
env.SHAREDLIB_GIT_ORG = env.SHAREDLIB_GIT_ORG ?: "cb-ci-templates"
env.SHAREDLIB_GIT_REPO = env.SHAREDLIB_GIT_REPO ?: "ci-shared-library"
env.SHAREDLIB_GIT_TAG_DEFAULT = env.SHAREDLIB_GIT_TAG_DEFAULT ?: "main" //"dev"
env.SHAREDLIB_GIT_CREDENTIALS = env.SHAREDLIB_GIT_CREDENTIALS ?: "ci-template-gh-app"

// Set the env.SHAREDLIB_GIT_TAG_ depending on whether env.SHAREDLIB_GIT_TAG is set or not on job/folder level.
// If not set, use default env.SHAREDLIB_GIT_TAG_DEFAULT
if (!env.SHAREDLIB_GIT_TAG) {
        echo "SHAREDLIB_GIT_TAG not set, loading shared lib default tag/branch ${env.SHAREDLIB_GIT_TAG_DEFAULT}"
        env.SHAREDLIB_GIT_TAG_ = env.SHAREDLIB_GIT_TAG_DEFAULT
} else {
        echo "Env var \$SHAREDLIB_GIT_TAG set on job/folder level, loading shared lib tag/branch ${env.SHAREDLIB_GIT_TAG}"
        env.SHAREDLIB_GIT_TAG_ = env.SHAREDLIB_GIT_TAG
}

library identifier: "${env.SHAREDLIB_GIT_REPO}@${env.SHAREDLIB_GIT_TAG_}", retriever: modernSCM(
        [$class: 'GitSCMSource',
         remote: "${env.SHAREDLIB_GIT_SERVER}/${env.SHAREDLIB_GIT_ORG}/${env.SHAREDLIB_GIT_REPO}.git",
         credentialsId: "${env.SHAREDLIB_GIT_CREDENTIALS}"
        ]
)

Map configMap = readYaml file: 'ci-config.yaml'
pipelineTemplateHelloWorld(configMap)
