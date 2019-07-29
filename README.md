# wildfirechat proto docker

[野火开源IM proto](https://github.com/wildfirechat/proto) docker编译环境  

docker build env for [wildfirechat IM proto](https://github.com/wildfirechat/proto)

## Installation

```
docker pull leondevlifelog/proto:latest
```

## Usage

example for jenkins pipeline
```
pipeline {
    agent {
        docker { 
            image 'leondevlifelog/proto:latest' 
            args '-v $HOME/.m2:/root/.m2'
        }
        
    }
    stages {
        stage('PullCode') {
            steps {
                //                           ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓      change to your proto code repo 
                git(branch:"firechat",url : "https://github.com/wildfirechat/proto.git")
            }
        }
        stage('BuildSoFile') {
            steps {
                dir("mars/libraries") {
                    sh "python2.7 build_android.py"
                }
            }
        }
        stage('BuildAAR'){
            steps {
                dir("mars/libraries/mars_android_sdk"){
                    sh "./gradlew build"
                }
            }
        }
        stage('Archive'){
            steps {
                archiveArtifacts artifacts: 'mars/libraries/mars_android_sdk/build/outputs/aar/*.aar', fingerprint: true
            }
        }
    }
}

```

example for bash  
```bash
coming soon...
```

## License

This project is licensed under the GPL-2.0 License - see the [LICENSE.md](LICENSE.md) file for details
