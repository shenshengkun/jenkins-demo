node('jnlp') {
    stage('Clone') {
        echo "1.Clone Stage"
        git url: "https://github.com/shenshengkun/jenkins-demo.git"
        script {
            build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
        }
    }
    stage('Test') {
      echo "2.Test Stage"
    }
    stage('Build') {
        echo "3.Build Docker Image Stage"
        sh "docker build -t registry.gag.cn/private/sy:${build_tag} ."
    }
    stage('Push') {
        echo "4.Push Docker Image Stage"
            sh "docker push registry.gag.cn/private/sy:${build_tag}"
    }
    stage('Deploy') {
        echo "5. Deploy Stage"
        if (env.BRANCH_NAME == 'master') {
            input "确认要部署线上环境吗？"
        }
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' k8s.yaml"
        sh "kubectl apply -f k8s.yaml --record"
    }
}
