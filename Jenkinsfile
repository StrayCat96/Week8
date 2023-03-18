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
      - name: gradle
        image: gradle:6.3-jdk14
        command:
        - sleep
        args:
        - 99d
        volumeMounts:
        - name: shared-storage
          mountPath: /mnt
      - name: kaniko
        image: gcr.io/kaniko-project/executor:debug
        command:
        - sleep
        args:
        - 9999999
        volumeMounts:
        - name: shared-storage
          mountPath: /mnt
        - name: kaniko-secret
          mountPath: /kaniko/.docker
      restartPolicy: Never
      volumes:
      - name: shared-storage
        persistentVolumeClaim:
          claimName: jenkins-pv-claim
      - name: kaniko-secret
        secret:
            secretName: dockercred
            items:
            - key: .dockerconfigjson
              path: config.json
''') { 
  node(POD_LABEL) {
  stage('Build a gradle project') {
      git 'https://github.com/dlambrig/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git' 
      container('gradle') {
          stage('Build a gradle project') {
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
  }
