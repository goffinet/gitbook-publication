pipeline {
  agent {
    docker {
      image 'goffinet/gitbook:latest'
      args '-v $WORKSPACE:/gitbook -v /root/.ssh:/root/.ssh'
    }

  }
  stages {
    stage('Install') {
      steps {
        sh 'gitbook install .'
      }
    }
    stage('Build ebooks') {
      parallel {
        stage('Build PDF') {
          steps {
            sh 'gitbook pdf . /opt/$EBOOK.pdf'
          }
        }
        stage('Build MOBI') {
          steps {
            sh 'gitbook mobi . /opt/$EBOOK.mobi'
          }
        }
        stage('Build EPUB') {
          steps {
            sh 'gitbook epub . /opt/$EBOOK.epub'
          }
        }
      }
    }
    stage('Publish') {
      steps {
        echo 'Ready to publish'
        sh 'scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -r /opt/$EBOOK.* $DESTPATH/'
      }
    }
  }
  environment {
    EBOOK = 'gitbook-publication'
    DESTPATH = 'root@get.goffinet.org:/var/www/html/gitbook-publication'
  }
}