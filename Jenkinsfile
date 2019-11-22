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
	/**stage("Test - dev") {
            steps { runUAT(8010) }
	}**/
	    
	stage("Approve") {
            steps { approve() }
	}
	
        stage("Deploy - Prod") {
            steps { buildApp('prod') }
	}

	/**stage("Test - UAT Live") {
            steps { runUAT(8020) }
	}**/
    }
}

def buildApp(environment) {

	if ("${environment}" == 'dev') {
		/**sh "docker stop \$(docker ps -q --filter name=librenms-dev)"
		sh "docker rm \$(docker ps -a -q --filter name=librenms-dev)"**/
		sh "cd dev ; docker-compose up -d --force-recreate ; cd .."
	} 
	else if ("${environment}" == 'prod') {
		/**sh "docker stop \$(docker ps -q --filter name=librenms-prod)"
		sh "docker rm \$(docker ps -a -q --filter name=librenms-prod)"**/
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
	sh "tests/runUAT.sh ${port}"

}

