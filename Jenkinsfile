#!/usr/bin/groovy

pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

	environment {
		PYTHONPATH = "${WORKSPACE}/librenms"
	}

    stages {

        stage("Build - dev") {
            steps { buildApp('dev') }
	}
	stage("Test - dev") {
            steps { runUAT(8010) }
	}
	    
	stage("Approve") {
            steps { approve() }
	}
	
        stage("Deploy - Prod") {
            steps { buildApp('prod') }
	}

	stage("Test - UAT Prod") {
            steps { runUAT(8020) }
	}
    }
}

def buildApp(environment) {

	if ("${environment}" == 'dev') {
		sh "docker ps -f name=librenms-dev -q | xargs --no-run-if-empty docker stop"
		sh "docker ps -a -f name=librenms-dev -q | xargs -r docker rm"
		sh "cd dev ; docker-compose up -d --force-recreate ; cd .."
	} 
	else if ("${environment}" == 'prod') {
		sh "docker ps -f name=librenms-prod -q | xargs --no-run-if-empty docker stop"
		sh "docker ps -a -f name=librenms-prod -q | xargs -r docker rm"
		sh "cd prod ; docker-compose up -d --force-recreate; cd .."
	}
	else {
		println "Environment not valid"
		System.exit(0)
	}
}

def approve() {

	timeout(time:1, unit:'DAYS') {
		input('Do you want to deploy to live?')
	}
}

def runUAT(port) {
	sh "chmod 755 tests/runUAT.sh"
	sh "tests/runUAT.sh ${port}"

}

