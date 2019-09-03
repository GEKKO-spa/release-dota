podTemplate(containers: [
  containerTemplate(name: 'helm', image: 'alpine/helm:latest', command: 'cat', ttyEnabled: true)
  ], 
  envVars: [
    podEnvVar(key:'CHART_NAME', value: 'chartmuseum/dotaki-score'), 
    podEnvVar(key:'NAMESPACE', value: 'dev'), 
  ]) {
  node(POD_LABEL){
    def test = 'export ohhl'
      withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_Dotaki_Preprod_Cred']]){
        stage('Git clone '){
          sh '''
          echo '######################## Deploy Prometheus Chart Start #################################'
          git clone https://github.com/loick-gekko/release-dota.git
          cd release-dota
          git checkout $BRANCH_NAME
          '''
        }
        stage('Deploy Prometheus Chart '){
          container('helm'){
            sh '''
            cd release-dota
            helm init --client-only
            helm repo update
            helm install $CHART_NAME --namespace $NAMESPACE --name $BRANCH_NAME -f values.yaml  || helm upgrade $BRANCH_NAME $CHART_NAME -f values.yaml
            echo '######################## Deploy node-workers End #################################'
            '''
          }
        }
      }
    }
}
