pipeline {
    agent any
    
    environment { 
        DEMO_PREPROD_URL = 'https://jao16384.sprint.dynatracelabs.com'
        DEMO_PREPROD_TOKEN = credentials('DEMO_PREPROD_TOKEN')
        DEMO_PROD_URL = 'https://fqp43822.live.dynatrace.com'
        DEMO_PROD_TOKEN = credentials('DEMO_PROD_TOKEN')
    }

    stages {
        stage('Debug Output') {
            steps {
                echo "------------------------"
                echo "DT Preprod URL: $DEMO_PREPROD_URL"
                echo "DT Prod URL: $DEMO_PREPROD_URL"
                echo "App Preprod URL: $PreprodURL"
                echo "App Preprod URL: $ProdURL"
                echo "Deploying project: $Project_Value to environment: $Environment_Value"
                echo "------------------------"
            }
        }
        stage ('Clone Monitoring Repository') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/Dynatrace-Adam-Gardner/monaco-demo'
            }
        }
        stage ('Download Monaco') {
            steps {
                sh 'wget --quiet https://github.com/dynatrace-oss/dynatrace-monitoring-as-code/releases/download/v1.5.3/monaco-linux-amd64 -O monaco'
                sh 'chmod +x monaco'
            }
        }
        stage ('Apply Dynatrace Configuration') {
            steps {
                script {
                    monaco_command = "./monaco --environments=environments.yaml"
                    if (env.Project_Value == "BaseConfig") monaco_command += " --project=projects/baseconfig"
                    if (env.Project_Value == "TeamA") monaco_command += " --project=projects/teama"
                    if (env.Environment_Value == "Preprod") monaco_command += " --specific-environment=preproduction"
                    if (env.Environment_Value == "Prod") monaco_command += " --specific-environment=production"
                }
                
                echo monaco_command
                sh monaco_command

            }
        }
    }
}
