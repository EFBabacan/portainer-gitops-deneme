pipeline {
  agent any
  environment {
    MESAJ = credentials('MESAJ')
    SURUM = 'v1'
  }
  stages {
    stage('Bilgi') {
      steps { sh 'echo "Surum: $SURUM | Secret: $MESAJ"' }
    }
    stage('Deploy') {
      steps {
        sh '''
cat > jenkins-compose.yml <<YAML
services:
  web:
    image: nginx:alpine
    restart: unless-stopped
    command:
      - /bin/sh
      - -c
      - echo "<h1>$SURUM - $MESAJ</h1>" > /usr/share/nginx/html/index.html && nginx -g 'daemon off;'
    ports:
      - "8092:80"
YAML
docker compose -p jenkins-nginx -f jenkins-compose.yml up -d --force-recreate
'''
      }
    }
  }
}
