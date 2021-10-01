# Jenins와 Docker를 활용한 CI/CD구축  

## Flow  
![](https://i.imgur.com/naNrLR9.png)

<br>

## 진행사항
### Docker  
1. docker 설치  


### Jenkins
1. docker에 jenkins 설치  
    * docker run 명령어  
    * 
        |option|help|  
        |-|-|  
        |-d|Detached 모드, 컨테이너가 백그라운드로 실행|  
        |-u|컨테이너 실행 시 리눅스 사용자 계정 이름 또는 UID를 설정|  
        |-p|특정 포트를 외부에 노출|  
        |-v|호스트와 공유할 디렉터리 설정하여 파일을 컨테이너에 저장하지 않고 호스트에 바로 저장|  

```
docker run -d -u root -p {포트번호}:8080 --name=jenkins -v c:/Users/{PC명}/dist:/var/jenkins_home/dist jenkins/jenkins 
```  

2. jenkins 접속
    2-1. chrome으로 jenkins 접속  
        ``http://localhost:{포트번호}`` 사용해서 접속  
    
    2-2. jenkins 로그인
        ID는 admin이고 ``docker logs jenkins``를 사용하면 아래처럼 비밀번호가 나온다.
        ![](https://i.imgur.com/EehBwGT.png)  
    2-3. 접속완료  
        ![](https://i.imgur.com/LcIWpIV.png)


3. Jenkins-Github 연동  
    3-1. Jenkins  
- ``DashBoard > Jenkins 관리 > 플로그인 관리 > 설치 가능``에서 NodeJS와 Github 관련된 플러그인을 설치한다.
- ``DashBoard > 새로운 Item > Pipeline`` 선택한다.  
    ![](https://i.imgur.com/CqN4TtD.png)  
            
- GitHub project와 연결한다.    
    ![](https://i.imgur.com/6jNOLK7.png)
    
- Pipeline을 작성한다.  
    ```script
    pipeline {
      agent any
      stages {
          stage('prepare') {
              steps {
                  git(poll: true, url: '{github repository}', branch: '브랜치명')
                }
            }

            stage('build') {
                steps {
                    dir(path: 'test') {
                        nodejs('jenkins-node') { 
                            sh 'npm install'
                            sh 'npm run build'
                            sh 'mv ./dist/* /var/jenkins_home/dist'
                        }
                    }
                }
            }
        }
    }
    ```


4. 끝!

    
