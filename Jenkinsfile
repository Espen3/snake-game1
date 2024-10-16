pipeline {
    agent none

    stages{
        stage('CLONE GIT REPOSITORY')
        {
            agent
            {
                label 'ubuntu-Appserver-1'
            }
            steps
            {
                checkout scm
            }
        }
        
        stage('SCA-SAST-SNYK-TEST')
        {
            agent
            {
                label 'ubuntu-Appserver-1'
            }
            steps
            {
                echo "SNYK-TEST"
            }
        }

        stage('BUILD-AND-TAG')
        {
            agent
            {
                label 'ubuntu-Appserver-1'
            }
            steps
            {
                script
                {
                    def app = docker.build("eapen303/Snake_game1")
                    app.tag('latest')
                }
            }
        }

        stage('PUSH-TO-DOCKERHUB')
        {
            agent
            {
                label 'ubuntu-Appserver-1'
            }
            steps
            {
                script
                {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_prop')
                    {
                        def app = docker.image("eapen303/Snake_game1")
                        app.push('latest')
                    }
                }
            }
        }

        stage('DEPLOYMENT')
        {
            agent
            {
                label 'ubuntu-Appserver-1'
            }
            steps
            {
                sh "docker-compose down"
                sh "docker-compose up -d"
            }
        }
    }
}