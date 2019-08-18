podTemplate(containers: [
  containerTemplate(name: 'helm', image: 'alpine/helm:latest', command: 'cat', ttyEnabled: true)
  ], 
  envVars: [
    podEnvVar(key:'CHART_NAME', value: 'chartmuseum/api-node-gekko'), 
    podEnvVar(key:'NAMESPACE', value: 'dev'), 
  ]) {
  node(POD_LABEL){
    def test = 'export ohhl'
      withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_Dotaki_Preprod_Cred']]){
        stage('Git clone '){
          sh '''
          echo '######################## Deploy node-workers Start #################################'
          git clone https://github.com/YannickGekko/dotaki-api-node.git
          cd dotaki-api-node
          git checkout $BRANCH_NAME
          '''
        }
        stage('Deploy api-node-gekko '){
          container('helm'){
            sh '''
            helm init --client-only
            helm repo add chartmuseum http://chartmuseum-chartmuseum.chartmuseum.svc.cluster.local:8080
            helm repo update
            helm install $CHART_NAME --namespace $NAMESPACE --name $BRANCH_NAME || helm upgrade $BRANCH_NAME $CHART_NAME
            echo '######################## Deploy node-workers End #################################'
            '''
          }
        }
      }
    }
}
