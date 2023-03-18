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
  stage('Build a gradle project') {
      git
'https://github.com/dlambrig/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git' 
    sh '''
    cd Chapter09/sample1
    chmod +x gradlew
    ./gradlew build
    mv ./build/libs/calculator-0.0.1-SNAPSHOT.jar /mnt
    '''
    }
    stage('k8s') {
      git
'https://github.com/dlambrig/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git' 
      container('centos') {
        stage('start calculator') { 
          sh '''
          cd Chapter09/sample1
          $ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://kubernetes.docker.internal:6443/api/v1/namespaces/devops-tools/pods" 
          $ chmod +x ./kubectl
          curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETE_SERVICE_PORT/apis/apps/v1/namespaces/devops-tools/deployments -XPOST -H "Content-type: application/yaml" --data-binary @hazelcast.yaml
          curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETE_SERVICE_PORT/apis/apps/v1/namespaces/devops-tools/deployments -XPOST -H "Content-type: application/yaml" --data-binary @calculator.yaml
            ''' 
            }
          }
        } 
        
          stage('build and push container image') {
    container('kaniko') {

    stage('build and upload container image') {
        sh '''
        echo 'FROM openjdk:8-jre' > Dockerfile
        echo 'COPY ./calculator-0.0.1-SNAPSHOT.jar app.jar' >> Dockerfile
        echo 'ENTRYPOINT ["java", "-jar", "app.jar"]' >> Dockerfile
        mv /mnt/calculator-0.0.1-SNAPSHOT.jar .
        /kaniko/executor --context `pwd` --destination straycat96/divcalc:1.0
        '''
        }

     }
    }
             
      }         
    }
