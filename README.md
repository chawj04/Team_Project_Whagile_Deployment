# TeamProject_Whagile_Deployment

![whagile](https://user-images.githubusercontent.com/97301076/191328749-e67c1594-5d77-43e6-80a4-c3cc5259f648.png)

### [ 팀 프로젝트를 따로 배포한 이유 ]<br>

배포 역할을 맡은 구성원이 있었으나, 프로젝트가 끝나고 `Docker`를 공부하면서 직접 배포해보고 싶었습니다.<br><br>

---

### [ 배포 하면서 겪은 이슈 및 해결 ]<br><br>

### 이슈 - `Nginx 403 forbidden` (client build)<br><br>

react-server를 Docker Image로 생성하려 했으나 해당 경로에 있는 build 파일에 접근할 수 없었습니다.<br><br>

![image](https://user-images.githubusercontent.com/97301076/191386540-1178f68d-40ea-4610-a6b8-50d0dbc6b082.png)<br>

![image](https://user-images.githubusercontent.com/97301076/191386487-feefb224-6969-4f4e-9e08-3d126b7113fb.png)

node 모듈과 build 파일 생성하고 `docker-compose`를 실행했음에도 여전히 build 파일에 대한 오류와 함께 화면은 403 에러를 보여주었습니다.<br>

권한을 주어 해결한 경우가 많은 것을 보고, chmod를 통해 해당 `nginx.conf`에 권한을 주었으나 변화는 없었습니다.<br><br><br>

### 해결 - `Nginx 403 forbidden` (client build)<br><br>

권한과 경로의 문제가 아니라면 ubuntu 위에서 build된 `파일 자체`가 문제 일 수 있겠다는 생각이 들었습니다. <br>
unbuntu에서 설치된 Node 모듈과 build 파일을 vscode에 있는 파일과 비교해보니 일부 파일이 `누락` 되어 설치된 것을 알게 되었습니다.<br><br>

![image](https://user-images.githubusercontent.com/97301076/191388761-498b2a79-5d3f-4361-a7e1-07b372727183.png)<br>

결국 vscode 상에서 client를 `직접 build한 파일`을 github에 추가함으로써 해결할 수 있었습니다. <br>
추가적으로, Git Clone으로 직접 내려받지말고 Docker Hub를 거쳐서 해결할 수 있지 않을까 생각하고 있습니다.
<br><br><br>

### 사소한 이슈 및 설정 <br><br>

- AWS 고정 IP 설정 하기 전에 build 하면, build된 파일 안에는 그 전에 설정한 IP로 build 됩니다. (주의)<br>

- nginx.conf 파일 내 - port와 server_name 변경 주의해야합니다.

- 인스턴스 인바운드 규칙 설정 잊지 말아야 합니다.<br>

- .env, docker-compose.yml, Dockerfile, nginx.conf 등은 ubuntu 환경에서 직접 설정하는 것이 좋습니다.<br>
  다만, 프로젝트 배포 내용을 확인하기 위해 GitHub에 올려두었습니다.<br>

#### `.env 파일만 .gitignore 한 이유`

- DB Server를 구름 IDE로 구축한 결과, 잦은 Ip 및 외부포트 변경이 있었습니다. (무료 이슈)<br>
  그래서 새롭게 Commit을 진행하고 프로젝트 전체를 내려받아 배포하는 상황이 생겼습니다.<br>
  이를 해결하기 위해 .env파일을 ubuntu 환경에서 직접 변경할 수 있도록 만들어 배포했습니다.

<br>
