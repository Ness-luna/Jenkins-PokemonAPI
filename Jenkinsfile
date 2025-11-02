pipeline {
  agent any

  stages {

    stage('Checkout') {
      steps {
        echo 'Clonando repositorio...'
        git branch: 'main', url: 'https://github.com/Ness-luna/Jenkins-PokemonAPI.git'
      }
    }

    stage('Build Image') {
      steps {
        echo 'Construyendo imagen Docker...'
        sh 'docker build -t pokemons-api:latest .'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Desplegando contenedor...'
        sh '''
          docker rm -f pokemons-api || true
          docker run -d --name pokemons-api -p 8081:8081 \
            -e NODE_ENV=production -e PORT=8081 pokemons-api:latest
          docker ps --filter name=pokemons-api
        '''
      }
    }
  }

  post {
    always {
      sh 'docker image prune -f || true'
    }
    success {
      echo 'OK...Pipeline completado correctamente.'
    }
    failure {
      echo 'Pipeline falló.'
    }
  }
}

