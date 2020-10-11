node {
    stage('Prepare') {
        echo 'Prepare...'
        checkout scm
        script {
            build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
            build_tag = "master-${build_tag}"
            image_name = "registry.cn-shanghai.aliyuncs.com/lian_ns/test_dj"
        }
    }
    stage('Test') {
        echo 'Testing...'
    }
    stage('Build') {
        echo 'Building...'
        sh "docker build -t ${image_name}:${build_tag} ."
    }
    stage('Deploy') {
        echo 'Deploying...'
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' ./k8s/deployment.yaml"
        sh "kubectl apply -f ./k8s/deployment.yaml --record"
        sh "kubectl apply -f ./k8s/ingress.yaml --record"
        sh "kubectl apply -f ./k8s/service.yaml --record"
    }
}
