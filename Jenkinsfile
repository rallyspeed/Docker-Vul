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

        stage("Build and Deploy") {
            steps { buildApp() }
	}
    }
	
    stages {

        stage("Test") {
            steps { testApp() }
	}
    }
}

def buildApp() {

	sh "docker-compose up -d --force-recreate"
}


def testApp() {

	sh "docker "
}

def approve() {

	timeout(time:1, unit:'DAYS') {
		input('Do you want to deploy to live?')
	}
}

def runUAT(port) {
	sh "tests/runUAT.sh ${port}"

}

