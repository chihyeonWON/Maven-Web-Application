# Maven-Web-Application
Maven-Web-Application by using apache tomcat

## 톰캣 매니저 디플로이어 사용

톰캣 서버의 매니저라는 디플로이어를 사용하도록 설정해야 한다.   
디플로이어(deployer)는 웹 브라우저를 통해 서버에 웹 애플리케이션을 쉽게 배포(deploy)할 수 있는 관리자라고 보면 된다.

Severs -> v8.0 Server at localhost-config 아래의 tomcat-users.xml 파일을 열어서 아래와 같이 추가한다.
![image](https://user-images.githubusercontent.com/58906858/180588389-762c4977-5168-4e8d-8b31-a2c3135e38f6.png)
역할과 유저를 등록하는 부분이다.   
rolename은 정해져 있는 값들인데 manager를 웹 화면에서 사용(manager-gui)하기 위한 역할과 메이븐 등의 스크립트에서 접근(manager-script)하기 위한 역할 등을 의미한다.   
user의 username과 password는 자유롭게 설정해도 된다.

이어서 Severs -> Tomcat v8.0 Server at localhost- config 아래 server.xml 파일을 열고 소스 마지막 부분에 아래와 같이 추가한다.
![image](https://user-images.githubusercontent.com/58906858/180588508-fb6b5cbd-d2d8-43f7-a09c-082b8bef8ab8.png)

Servers -> Tomcat v8.0 Server at localhost -config를 다음과 같이 실행하고
![image](https://user-images.githubusercontent.com/58906858/180588798-ad6de714-d712-4e76-88a4-b15ef2d95416.png)

브라우저를 열고 http://127.0.0.1:8080/manager를 입력하고 사용자 이름과 비밀번호를 tomcat-users.xml파일에서 설정한대로 입력하여 로그인한다.
![image](https://user-images.githubusercontent.com/58906858/180589000-fd8f9abf-9787-43b3-a99e-603405859b15.png)

아래와 같이 뜨면 톰캣 매니저 디플로이어 접속이 완료되었다.
![image](https://user-images.githubusercontent.com/58906858/180589199-bbd706a3-9019-4e4d-9a9f-6a48dd9fef20.png)


## Tomcat http 404 error 문제 발생과 해결방법

브라우저를 열고 http://127.0.0.1:8080/manager를 입력했을 때 다음과 같은 화면이 뜨는 경우가 종종 발생한다.
![image](https://user-images.githubusercontent.com/58906858/180588942-0ffa5f61-a859-419e-b64e-5b53acd04ada.png)

이때는 Server탭의 서버 더블클릭후 OverView에서 ServerLocation - Use Tomcat Installation 클릭후 저장한다.
![image](https://user-images.githubusercontent.com/58906858/180588977-677f45ef-9534-42c4-9540-192ade8eb2bc.png)

프로젝트의 properties의 web path 경로도 servers.xml에서 지정해준것과 같게 되어있는지 다시 확인해본다.

http 404 문제 해결완료..
