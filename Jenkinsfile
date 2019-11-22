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
