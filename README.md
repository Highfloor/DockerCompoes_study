# compose_awesome
# 개요

- 정의
  - 시스템 구축과 같은 관련된 작업(명령어들)을 하나의 작업 파일(정의 파일)에 기재
- 특징
  - 명령어 한번에 시스템 전체를 실행/종료/페기가지 한번에 수행하도록 도와주는 도구
  - 생상선 향상, 유지보수 유리

```
Web 3-tier
-----------------------
network create (web<->was, was<->db, ..)
+
volume create
+ 
docker run (db:mariadb)
+
docker run (was:python + flask)
+
docker run (web:nginx or apache)
-----------------------
정의파일 1개로 구성(단, 개별 이미지에 대한 Dockerfile은 존재할수 있다) => *.yml + Dockerfile + 리소스

```
- 여러개의 docker run 명령어를 묶어놓은것과 유사

-   <img src="https://firebasestorage.googleapis.com/v0/b/repo-27c12.appspot.com/o/docker%2Fdocker_compose_vs_dockerfile.png?alt=media&token=a0e287d9-030c-44ae-b595-be363fe445fb" width="500">

- 컨테이너, 주변환경(볼륨, 네트워크)까지 모두 구성
  - 비교 : 쿠버네티스는 컨테이너를 관리하는 도구

# 정의 파일 및 역활

<img src="https://firebasestorage.googleapis.com/v0/b/repo-27c12.appspot.com/o/docker%2Fdocker_compose_yml.png?alt=media&token=7588398e-a49a-4874-bf4e-16ff8be1d5d7" width="500">

- docker-compose up -d
  - 정의된 파일 내용대로 컨테이너, 네트워크, 볼륨등 이미지다운로드(없다면), 생성, 가동
- docker-compose down
  - 컨테이너만 삭제
  - 이미지, 네트워크, 볼륨 남아 있다
  
<img src="https://firebasestorage.googleapis.com/v0/b/repo-27c12.appspot.com/o/docker%2Fdocker_compose_up_down.png?alt=media&token=af95f657-4714-43bd-a219-5ecc9666c10b" width="500">  

- 파일명
  - docker-compose.yml
  - 이름이 다르면 -f 옵션
    - 단, 통상 같은 이름 사용

- 규칙
  - 폴더안에는 오직 한개의 정의 파일만 존재할 수 있다

- CLi 방식과 컴포즈 방식의 차이점
  - CLI는 모든것은 개발자가 명령어로 모두 지시
  - 컴포즈 정의 파일에 모든 내용을 기술하고, 명령 한번이면 구성이 완료

## 정의 파일 구성

- 형식
  - YAML 형식
  - 확장자, yaml, yml
  - 텍스트 에디터에서 작업
    - 주의사항
      - 들여쓰기(오와 열)를 정확하게 지킨다
      - 같은 서열끼지는 같은 공백의 양을 가진다
      - 탭 사용 불가(일반적으로)
      - 검수 체크 1순위는 공백이 모두 동일한지
- 파일명 위에 내용 참고
- 주항목 (대분류)
  - version 
    - 컴포즈 버전
  - services
    - 컨테이너 관련 내용 기술
  - networks
    - 네트워크
  - volumes
    - 볼륨

- 주의사항
  - 항목이름 뒤에는 반드시 `:` 사용
  - `:` 뒤에는 반드시 공백 한개 존재, 값을 넣는다면
  <img src="https://firebasestorage.googleapis.com/v0/b/repo-27c12.appspot.com/o/docker%2Fdocker_compose_%E1%84%83%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%A7%E1%84%8A%E1%85%B3%E1%84%80%E1%85%B51.png?alt=media&token=ff8a0be1-460b-4dc0-9a4e-6fcfa7653822">

- YAML 작성 요령
  - <img src="https://firebasestorage.googleapis.com/v0/b/repo-27c12.appspot.com/o/docker%2Fdocker_compose_%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%B2%E1%84%8E%E1%85%B5%E1%86%A81.png?alt=media&token=3fa3303a-3035-4f37-b1f8-956727ecedae">

- 대분류 다음에 나오는 부항목(중분류)
  - <img src="https://firebasestorage.googleapis.com/v0/b/repo-27c12.appspot.com/o/docker%2Fdocker_compose_%E1%84%8C%E1%85%A1%E1%84%8C%E1%85%AE%E1%84%82%E1%85%A1%E1%84%8B%E1%85%A9%E1%84%82%E1%85%B3%E1%86%AB%E1%84%87%E1%85%AE%E1%84%92%E1%85%A1%E1%86%BC%E1%84%86%E1%85%A9%E1%86%A81.png?alt=media&token=611d314c-eba0-4fa0-896e-450ae743183a">
  - <img src="https://firebasestorage.googleapis.com/v0/b/repo-27c12.appspot.com/o/docker%2Fdocker_compose_%E1%84%8C%E1%85%A1%E1%84%8C%E1%85%AE%E1%84%82%E1%85%A1%E1%84%8B%E1%85%A9%E1%84%82%E1%85%B3%E1%86%AB%E1%84%87%E1%85%AE%E1%84%92%E1%85%A1%E1%86%BC%E1%84%86%E1%85%A9%E1%86%A82.png?alt=media&token=8f43c400-e372-47ed-bf02-822aff6a6081">
  - dependon
    - 예시 : db -> flask -> web sevrer
  - restart
    - 컨테이너 종료시 재가동 여부, 특이 사항 없으면 재가동하는것으로 설정
    - <img src="https://firebasestorage.googleapis.com/v0/b/repo-27c12.appspot.com/o/docker%2Fdocker_compose_restart%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC.png?alt=media&token=52cc30e7-217e-493c-b8d1-7e0385f1f9c9">

- 자주 나오는 부항목
- <img src="https://firebasestorage.googleapis.com/v0/b/repo-27c12.appspot.com/o/docker%2Fdocker_compose_%E1%84%80%E1%85%B5%E1%84%90%E1%85%A1%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B4%E1%84%92%E1%85%A1%E1%86%BC%E1%84%86%E1%85%A9%E1%86%A8.png?alt=media&token=dc3d64c0-02a4-4d5f-8cc5-4c5a20ae1b59">

# 구축실습

## Flask 

- 파일구조
  ```
    flask
    L readme.md
    L app
        L Dockerfile            # flask 컨테이너 이미지를 기술한 파일
        L app.py                # flask의 엔트리포인트
        L requirements.txt      # flask 구동을 위한 패키지 설치파일
    L docker-compose.yml        # 컴포즈 파일 (호환된다 compose.yml)
  ```

- 명령어
  - 해당 프로젝트 폴더로 이동
    - cd flask
  - 컴포드 빌드
    - docker-compose up -d
  - 구동 확인
    - http://127.0.0.1:8000
  - 컨테이너 접속 (알파인 리눅스라서 쉘프로그램 이름이 다름 : ash)
    - docker exec -it flask-was-1 ash
  - 컨테이너 삭제
    - docker-compose down
    ```
     - Container flask-was-1  Removed                                                                                1.3s
     - Network flask_default  Removed                                                                                0.8s
    ```
  - 이미지 조회
    - docker image ls
    ```
      REPOSITORY                  TAG                    IMAGE ID       CREATED         SIZE
      flask-was                   latest                 1d99e7886dd0   3 hours ago     60.5MB
    ```
  - 이미지 삭제
    - docker image rm flask-was

## 3-tire


  

- 웹 어플리케이션의 기본구조
- Web + Was + DB
  - Web : nginx, 설정추가(가상 호스트를 넣어서 was와 연동을 추가), 80요청->8000번 포워딩 처리, 도메인연결(생략), https 고려
  - Was : flask, DB 연동, 단독 세팅과 유사함
  - db  : maridb, 환경설정, db가 살아있는지 주기적으로 체크하는 신호 설정
- 네트워크 
  - Web + Was
  - Was + db 
- 볼륨
  - 1개 혹은 1개 이상
  - 볼륨 마운트

- 1개의 물리적 서버에서 3개의 컨테이너를 구성하겠다
  - 3개의 물리적 서버 + 1개 public Subnet, 2개의 private subnet 구성해서 서비스
  - 향후 오세스트레이션 기술을 적용한 쿠버네티스(EKS) or aws ECS 에서는 n개의 물리적서버에 n개의 컨테이너를 운영하는 방식으로 구성
