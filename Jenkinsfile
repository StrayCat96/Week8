podTemplate(yaml: ''' 
    apiVersion: v1
    kind: Pod 
    spec:
      containers:
      - name: centos
        image: centos 
        command:
        - sleep
        args:
        - 99d 
      restartPolicy: Never
''') { 
  node(POD_LABEL) {
    stage('k8s') {
      git
'https://github.com/dlambrig/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git' 
      container('centos') {
        stage('start calculator') { 
          sh '''
          cd Chapter09/sample1
          curl -ik -H "Authorization: Bearer $(cat 
/var/run/secrets/kubernetes.io/serviceaccount/token)" 
https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/api/v1/namespaces/devops-tools/pods
curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETE_SERVICE_PORT/apis/apps/v1/namespaces/default/deplo yments -XPOST -H "Content-type: application/yaml" --data-binary @hazelcast.yaml
curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETE_SERVICE_PORT/apis/apps/v1/namespaces/default/deplo yments -XPOST -H "Content-type: application/yaml" --data-binary @calculator.yaml
test $(curl calculator-service:8080/sum?a=6\\&b=2) -eq 3 && echo 'pass' || 'fail
                  ''' 
                  }
                } 
              }
              
    }
}
