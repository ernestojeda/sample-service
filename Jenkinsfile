//
// Copyright (c) 2019 Intel Corporation
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
//
//      http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.
//
@Library("edgex-global-pipelines@6099e5c6ab04cb973443b77e0e33e555f9711d3e") _

pipeline {
    agent any
    options {
        timestamps()
    }
    environment {
        SEMVER_PRE_PREFIX = 'dev'
        DRY_RUN = 'false'
        //SEMVER_BRANCH = 'semver-testing'
    }
    stages {
        stage('Git Semver') {
            steps {
                script {
                    def version = edgeXSemver('init')
                    println "semver version is ${version}"
                    edgeXSemver('tag')
                    edgeXSemver('bump pre')
                    edgeXSemver('push')
                    sh 'env'
                    env.GITSEMVER_HEAD_TAG = ''
                }
            }
        }
        stage('Git Semver - Repeated') {
            steps {
                script {
                    def version = edgeXSemver('init')
                    println "semver version is ${version}"
                    edgeXSemver('tag')
                    edgeXSemver('bump pre')
                    edgeXSemver('push')
                    sh 'env'
                    env.GITSEMVER_HEAD_TAG = ''
                }
            }
        }
        stage('Build Commit') {
            steps {
                script {
                    def version = edgeXSemver('init', '4.1.8')
                    println "semver version is ${version}"
                    edgeXSemver('tag -force')
                    edgeXSemver('bump pre')
                    edgeXSemver('push')
                    sh 'env'
                    env.GITSEMVER_INIT_VERSION = ''
                }
            }
        }
        stage('Build Commit - Repeated') {
            steps {
                script {
                    def version = edgeXSemver('init', '4.1.8')
                    println "semver version is ${version}"
                    edgeXSemver('tag -force')
                    edgeXSemver('bump pre')
                    edgeXSemver('push')
                    sh 'env'
                    env.GITSEMVER_INIT_VERSION = ''
                }
            }
        }
        stage('Release') {
            steps {
                script {
                    def releaseInfo = [:]
                    releaseInfo['name'] = 'sample-service'
                    releaseInfo['version'] = '4.1.8'
                    releaseInfo['repo'] = 'https://github.com/edgexfoundry/sample-service.git'
                    releaseInfo['releaseStream'] = 'master'
                    releaseInfo['gitTag'] = true

                    edgeXReleaseGitTag(releaseInfo)
                }
            }
        }
    }
}
