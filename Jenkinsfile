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
}

def buildApp() {

	sh "docker-compose up -d --force-recreate"
}
