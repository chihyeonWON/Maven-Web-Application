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
![image](https://user-images.githubusercontent.com/58906858/180631016-b07a6d70-533f-4025-9f7b-248bc819f446.png)

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

## 메이븐 웹 애플리케이션

서버에 배포할 메이븐 웹 애플리케이션을 생성한다.   
New -> Other -> Maven -> Maven Project 를 선택하고 maven-archetype-webapp을 선택하여 웹 애플리케이션을 만든다.      
![image](https://user-images.githubusercontent.com/58906858/180630328-ec0ba928-6b62-4983-a98a-5959373b472f.png)

생성된 maven_web이라는 애플리케이션은 이클립스에서 Target Runtime으로 서버 설정이 안 되어 있어 에러가 발생한다. 에러는 무시하고 마지막에 에러를 없애는 방법을 정리한다.   

## 메이븐 톰캣 플러그인 설정

메이븐 프로젝트를 톰캣 서버에 배포하기 위해서는 톰캣 플러그인을 추가해야 한다.   
프로젝트 마우스 오른쪽 클릭 후 Maven -> Add Plugin을 선택한다.      
![image](https://user-images.githubusercontent.com/58906858/180630500-1ade1992-8bb2-44f2-a23c-fe8d157b8d02.png)

이렇게 하면 pom.xml 파일에 톰캣 플러그인이 추가된 것을 확인할 수 있다.   
![image](https://user-images.githubusercontent.com/58906858/180630509-b03db957-b1de-4a71-bf84-1f409e8f7e47.png)

tomcat7-maven-plugin 플러그인 태그 <version> 밑에 configuration, url, username, password 등을 추가하여 작성한다.   
유저와 패스워드 정보는 tomcat-users.xml 파일에 기술된 값을 사용한다.   
![image](https://user-images.githubusercontent.com/58906858/180630659-7152c892-2190-4291-b805-5e95f59757cb.png)

Servers -> Tomcat v8.0 Server at localhost -config 를 실행해 서버를 구동한다.   
서버가 정상적으로 구동되면, 웹 애플리케이션에서 Run As -> Maven Build를 수행한다.

실행할 플러그인의 Goal은 tomcat7:deploy로 입력한다.   
![image](https://user-images.githubusercontent.com/58906858/180630692-c3c1a82e-fc55-4a84-93ff-f4d81c2f63e2.png)

콘솔 뷰에서 빌드가 성공했는지 메시지를 확인한다.   
![image](https://user-images.githubusercontent.com/58906858/180630719-17fb9183-d57a-4ce7-a755-57184bd19354.png)

  
C:\eGovFrame-3.10.0\bin\eclipse\.metadata\.plugins\org.eclipse.wst.server.core\tmp1\webapps 경로에서 maven_web.war가 배포된 것을 확인할 수 있다.   
![image](https://user-images.githubusercontent.com/58906858/180631406-d91fb497-571b-4e60-a61c-fd2e894ac760.png)
파일을 찾아가는 경로는 Servers -> Tomcat v8.0 Server at localhost-config를 더블클릭하면 설정화면에서 확인가능하다.   

브라우저를 실행하고 http://127.0.0.1:8080/maven_web/를 입력하여 결과를 확인한다.   
![image](https://user-images.githubusercontent.com/58906858/180631313-4186510f-db2a-4131-b299-121d41658b01.png)

Goal의 tomcat7:undeploy나 tomcat7:redeploy 등을 수행해보고 디렉터리의 변화를 살펴본다.   
tomcat7:undeploy를 수행하면 배포되었던 애플리케이션이 삭제된다.   

## 이클립스 Target Runtime 오류 발생과 문제 해결

이클립스에서 컴파일하고 실행시키는 것이 아니므로 에러와 상관없이 실행되지만 에러를 없애고 싶다면 프로젝트를 선택하고 마우스 오른쪽 클릭 후   
properties -> Target Runtime -> Apache Tomcat v8.0 을 체크한다.
![image](https://user-images.githubusercontent.com/58906858/180631428-6e1973ba-fd58-4c60-ad56-ad641a09cef8.png)

웹 애플리케이션을 실행시키기 위해 필요한 라이브러리 패스가 추가된다.   
![image](https://user-images.githubusercontent.com/58906858/180631451-a3dee505-74cd-40da-9a62-8dac824a4ec6.png)

