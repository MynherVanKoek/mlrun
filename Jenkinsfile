label = "${UUID.randomUUID().toString()}"
git_project = "mlrun"
git_project_user = "mlrun"
git_project_upstream_user = "mlrun"
git_deploy_user = "iguazio-prod-git-user"
git_deploy_user_token = "iguazio-prod-git-user-token"
git_deploy_user_private_key = "iguazio-prod-git-user-private-key"

podTemplate(label: "${git_project}-${label}", inheritFrom: "jnlp-docker-golang-python37") {
    node("${git_project}-${label}") {
        pipelinex = library(identifier: 'pipelinex@development', retriever: modernSCM(
                [$class       : 'GitSCMSource',
                 credentialsId: git_deploy_user_private_key,
                 remote       : "git@github.com:iguazio/pipelinex.git"])).com.iguazio.pipelinex
        common.notify_slack {
            withCredentials([
                    string(credentialsId: git_deploy_user_token, variable: 'GIT_TOKEN')
            ]) {
                github.release(git_deploy_user, git_project, git_project_user, git_project_upstream_user, true, GIT_TOKEN) {
                    parallel(
                        "build ${git_project}/api in dood": {
                            container('docker-cmd') {
                                stage("build ${git_project}/api in dood") {
                                    dir("${github.BUILD_FOLDER}/src/github.com/${git_project_upstream_user}/${git_project}") {
                                        common.shellc("export MLRUN_DOCKER_TAG=${github.DOCKER_TAG_VERSION} && make api")
                                    }
                                }
                                stage('push api') {
                                    dockerx.images_push_multi_registries(["${git_project}/mlrun-api:${github.DOCKER_TAG_VERSION}"], [pipelinex.DockerRepo.ARTIFACTORY_IGUAZIO, pipelinex.DockerRepo.DOCKER_HUB, pipelinex.DockerRepo.QUAY_IO])
                                }
                            }
                        },
                        "build ${git_project}/base in dood": {
                            container('docker-cmd') {
                                stage("build ${git_project}/base in dood") {
                                    dir("${github.BUILD_FOLDER}/src/github.com/${git_project_upstream_user}/${git_project}") {
                                        common.shellc("export MLRUN_DOCKER_TAG=${github.DOCKER_TAG_VERSION} && make base")
                                    }
                                }
                                stage('push base') {
                                    dockerx.images_push_multi_registries(["${git_project}/ml-base:${github.DOCKER_TAG_VERSION}"], [pipelinex.DockerRepo.ARTIFACTORY_IGUAZIO, pipelinex.DockerRepo.DOCKER_HUB, pipelinex.DockerRepo.QUAY_IO])
                                }
                            }
                        },
                        "build ${git_project}/base-legacy in dood": {
                            container('docker-cmd') {
                                stage("build ${git_project}/base-legacy in dood") {
                                    dir("${github.BUILD_FOLDER}/src/github.com/${git_project_upstream_user}/${git_project}") {
                                        common.shellc("export MLRUN_DOCKER_TAG=${github.DOCKER_TAG_VERSION} && make base-legacy")
                                    }
                                }
                                stage('push base-legacy') {
                                    dockerx.images_push_multi_registries(["${git_project}/ml-base:${github.DOCKER_TAG_VERSION}-py36"], [pipelinex.DockerRepo.ARTIFACTORY_IGUAZIO, pipelinex.DockerRepo.DOCKER_HUB, pipelinex.DockerRepo.QUAY_IO])
                                }
                            }
                        },
                        "build ${git_project}/models in dood": {
                            container('docker-cmd') {
                                stage("build ${git_project}/models in dood") {
                                    dir("${github.BUILD_FOLDER}/src/github.com/${git_project_upstream_user}/${git_project}") {
                                        common.shellc("export MLRUN_DOCKER_TAG=${github.DOCKER_TAG_VERSION} && make models")
                                    }
                                }
                                stage('push models') {
                                    dockerx.images_push_multi_registries(["${git_project}/ml-models:${github.DOCKER_TAG_VERSION}"], [pipelinex.DockerRepo.ARTIFACTORY_IGUAZIO, pipelinex.DockerRepo.DOCKER_HUB, pipelinex.DockerRepo.QUAY_IO])
                                }
                            }
                        },
                        "build ${git_project}/models-legacy in dood": {
                            container('docker-cmd') {
                                stage("build ${git_project}/models-legacy in dood") {
                                    dir("${github.BUILD_FOLDER}/src/github.com/${git_project_upstream_user}/${git_project}") {
                                        common.shellc("export MLRUN_DOCKER_TAG=${github.DOCKER_TAG_VERSION} && make models-legacy")
                                    }
                                }
                                stage('push models-legacy') {
                                    dockerx.images_push_multi_registries(["${git_project}/ml-models:${github.DOCKER_TAG_VERSION}-py36"], [pipelinex.DockerRepo.ARTIFACTORY_IGUAZIO, pipelinex.DockerRepo.DOCKER_HUB, pipelinex.DockerRepo.QUAY_IO])
                                }
                            }
                        },
                        "build ${git_project}/models-gpu in dood": {
                            container('docker-cmd') {
                                stage("build ${git_project}/models-gpu in dood") {
                                    dir("${github.BUILD_FOLDER}/src/github.com/${git_project_upstream_user}/${git_project}") {
                                        common.shellc("export MLRUN_DOCKER_TAG=${github.DOCKER_TAG_VERSION} && make models-gpu")
                                    }
                                }
                                stage('push models-gpu') {
                                    dockerx.images_push_multi_registries(["${git_project}/ml-models-gpu:${github.DOCKER_TAG_VERSION}"], [pipelinex.DockerRepo.ARTIFACTORY_IGUAZIO, pipelinex.DockerRepo.DOCKER_HUB, pipelinex.DockerRepo.QUAY_IO])
                                }
                            }
                        },
                        "build ${git_project}/models-gpu-legacy in dood": {
                            container('docker-cmd') {
                                stage("build ${git_project}/models-gpu-legacy in dood") {
                                    dir("${github.BUILD_FOLDER}/src/github.com/${git_project_upstream_user}/${git_project}") {
                                        common.shellc("export MLRUN_DOCKER_TAG=${github.DOCKER_TAG_VERSION} && make models-gpu-legacy")
                                    }
                                }
                                stage('push models-gpu-legacy') {
                                    dockerx.images_push_multi_registries(["${git_project}/ml-models-gpu:${github.DOCKER_TAG_VERSION}-py36"], [pipelinex.DockerRepo.ARTIFACTORY_IGUAZIO, pipelinex.DockerRepo.DOCKER_HUB, pipelinex.DockerRepo.QUAY_IO])
                                }
                            }
                        },
                        "build ${git_project}/mlrun in dood": {
                            container('docker-cmd') {
                                stage("build ${git_project}/mlrun in dood") {
                                    dir("${github.BUILD_FOLDER}/src/github.com/${git_project_upstream_user}/${git_project}") {
                                        common.shellc("export MLRUN_DOCKER_TAG=${github.DOCKER_TAG_VERSION} && make mlrun")
                                    }
                                }
                                stage('push mlrun') {
                                    dockerx.images_push_multi_registries(["${git_project}/mlrun:${github.DOCKER_TAG_VERSION}"], [pipelinex.DockerRepo.ARTIFACTORY_IGUAZIO, pipelinex.DockerRepo.DOCKER_HUB, pipelinex.DockerRepo.QUAY_IO])
                                }
                            }
                        }
                    )
                }
            }
        }
    }
}
