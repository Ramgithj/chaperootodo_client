pipeline{
        agent any
        environment {
                DB_PASSWORD=credentials('DATABASE_PASSWORD')
        }
        stages{
                stage ("Clone git") {
                        steps {
                               sh ' chmod +x cloneproject'
                               sh './cloneproject'
                        }
                }
                stage ("install Docker and docker-compose") {
                        steps {
                                sh 'sudo apt install -y curl jq'
                                sh 'curl https://get.docker.com | sudo bash'
                                sh 'sudo usermod -aG docker $(whoami)'
//                                sh 'sudo apt update'
  //                              sh 'sudo apt install -y curl jq'
                                sh "version=\$5(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r '.tag_name')"
                                sh 'sudo curl -L "https://github.com/docker/compose/releases/download/${version}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose'
                                sh 'sudo chmod +x /usr/local/bin/docker-compose'
                        }
                }
                stage ("deploy") {
                        steps {
                                sh 'sudo docker-compose pull && sudo -E DB_PASSWORD=${DB_PASSWORD} docker-compose up -d'
                        }
                }
        }
}
