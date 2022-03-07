# Setting JOTA Site

*Last Update: 14. Jul. 2021*

**DMOJ site official docs**: https://docs.dmoj.ca/#/site/installation

## 시작하기 전 필독

* 반드시 공식 문서를 읽으면서 진행하십시오. 명령어는 아래의 수정된 명령어를 사용하세요.
* 본인의 github 계정에서 해당 repository를 **fork** 하십시오: https://github.com/hyunchan-park/JOTA-dmoj-online-judge.
* pip3 패키지 설치 또는 python3 코드 실행 중 오류가 발생할 때, 접두어 `sudo` 를 붙이지 않았는지 확인하십시오.
* 패키지의 버전 업데이트로 인하여 세팅이 실패될 수 있습니다.

## Installing the prerequisites
original: https://docs.dmoj.ca/#/site/installation?id=installing-the-prerequisites

* 각종 패키지 최신 업데이트 및 설치
    ```
    $ sudo apt update
    $ sudo apt install git gcc g++ make python3-dev python3-pip libxml2-dev libxslt1-dev zlib1g-dev gettext curl redis-server
    $ sudo curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
    $ sudo apt install nodejs
    $ sudo npm install -g sass postcss-cli postcss autoprefixer
    $ sudo apt install python3.8-venv
    ```

## Creating the database
original: https://docs.dmoj.ca/#/site/installation?id=creating-the-database

* 최신 버전의 mariadb-server 와 libmysqlclient-dev 패키지 설치
    ```
    $ sudo apt update
    $ sudo apt install mariadb-server libmysqlclient-dev
    ```
* 데이터베이스 생성하기
    ```
    $ sudo mysql -uroot -p
    Enter password: <ubuntu 비밀번호>
    mariadb> CREATE DATABASE dmoj DEFAULT CHARACTER SET utf8mb4 DEFAULT COLLATE utf8mb4_general_ci;
    mariadb> GRANT ALL PRIVILEGES ON dmoj.* to 'dmoj'@'localhost' IDENTIFIED BY '<DB-비밀번호>';
    mariadb> exit
    ```

## Installing prerequisites
앞으로 진행할과정에서 8001, 9999 포트 번호를 사용합니다. 충돌이 나지 않게 주의해주세요.
original: https://docs.dmoj.ca/#/site/installation?id=installing-prerequisites

* jota 디렉토리 생성 및 이동

  ```
  ~$ mkdir ~/jota
  ~$ cd ~/jota
  ```

* 가상환경 구축 및 실행
    ```
    ~/jota$ python3 -m venv dmojsite
    ~/jota$ . ~/jota/dmojsite/bin/activate
    ```
    
* fork 했던 저장소를 `site` 폴더 안에 저장
    ```
    (dmojsite) ~/jota$ git clone https://github.com/<your-github-id>/JOTA-dmoj-online-judge site
    (dmojsite) ~/jota$ cd site
    (dmojsite) ~/jota/site$ git submodule init
    (dmojsite) ~/jota/site$ git submodule update
    ```
    
* python3 관련 패키지 모두 설치
    ```
    (dmojsite) ~/jota/site$ sudo pip3 install -r requirements.txt
    (dmojsite) ~/jota/site$ sudo pip3 install mysqlclient
    (dmojsite) ~/jota/site$ sudo pip install --upgrade jinja2
    ```
    
    
* curl 명령어로 `local_settings.py` 파일 다운로드 (dmojsite가 activate되지 않아도 할 수 있는 과정)
  ```
  ~/jota/site$ curl -o dmoj/local_settings.py https://raw.githubusercontent.com/DMOJ/docs/master/sample_files/local_settings.py
  ```

  + vi 명령어 혹은 Code-Server 상에서 `~/jota/site/dmoj/local_settings.py` 파일 수정

    ```
    ~/jota/site$ vi dmoj/local_settings.py
    ```
   

  * 할당받은 외부IP 이용 시 ALLOWED_HOSTS 항목 수정

      ```
      ALLOWED_HOSTS = ['<IP-Address>']
      ```
    할당받은 외부 IP는 JCLOUD에서 따로 받은 floating ip을 의미함
    
  * DATABASES 내 PASSWORD 항목 수정

      ```db
      DATABASES = {
          'default': {
              'ENGINE': 'django.db.backends.mysql',
              'NAME': 'dmoj',
              'USER': 'dmoj',
              'PASSWORD': '<DB-비밀번호>',
              'HOST': '127.0.0.1',
              'OPTIONS': {
                  'charset': 'utf8mb4',
                  'sql_mode': 'STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION',
              },
      ```

  * 로그파일 절대경로 설정 (e.g. '/home/ubuntu/jota/logfile.txt')

    ```
    'handlers': {
        # You may use this handler as example for logging to other files..
        'bridge': {
            ...
            'filename': '/home/ubuntu/jota/logfile.txt',
            ...
        },
    ```

* `manage.py` 실행
    
    ```
    (dmojsite) ~/jota/site$ sudo python3 manage.py check
    System check identified no issues (47 silenced).
    ```

## Compiling assets
original: https://docs.dmoj.ca/#/site/installation?id=compiling-assets

* 스타일 시트 생성
    ```
    (dmojsite) ~/jota/site$ ./make_style.sh
    ```
* 정적 파일 수집 (`dmoj/local_settings.py`의 환경변수 `STATIC_ROOT` 참고)
    ```
    (dmojsite) ~/jota/site$ sudo python3 manage.py collectstatic
    ```
* 국가별 언어 관련 파일 생성
    ```
    (dmojsite) ~/jota/site$ sudo python3 manage.py compilemessages
    (dmojsite) ~/jota/site$ sudo python3 manage.py compilejsi18n
    ```

## Setting up database tables
original: https://docs.dmoj.ca/#/site/installation?id=setting-up-database-tables
* 데이터베이스 스키마 생성
    ```
    (dmojsite) ~/jota/site$ sudo python3 manage.py migrate
    ```
* 초기 데이터 로드
    ```
    (dmojsite) ~/jota/site$ sudo python3 manage.py loaddata navbar
    (dmojsite) ~/jota/site$ sudo python3 manage.py loaddata language_small
    (dmojsite) ~/jota/site$ sudo python3 manage.py loaddata demo
    ```
* admin 계정 만들기
    ```
    (dmojsite) ~/jota/site$ sudo python3 manage.py createsuperuser
    ```
    굳이 위 명령을 실행하지 않아도, `ID = admin, PW = admin` 의 관리자 계정이 만들어져 있습니다.
## Setting up Celery
original: https://docs.dmoj.ca/#/site/installation?id=setting-up-celery

* 가상환경 해제

  ```
  (dmojsite) ~/jota/site$ deactivate
  ~/jota/site$ cd ~
  ```

* Celery workers 인증
    ```
    $ service redis-server start
    
    ==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
    Authentication is required to start 'redis-server.service'.
    Authenticating as: Ubuntu (ubuntu)
    Password: 
    ==== AUTHENTICATION COMPLETE ===
    ```

## Running the server
original: https://docs.dmoj.ca/#/site/installation?id=running-the-server


1. 웹 서버를 열기 위한 새 SSH 연결 세션을 만듭니다. (세션 #1)
    ```
    ~$ . ~/jota/dmojsite/bin/activate
    (dmojsite) ~$ sudo python3 ~/jota/site/manage.py runserver 0.0.0.0:8001
    ```

2. bridged를 실행하기 위한 새 SSH 연결 세션을 만듭니다. (세션 #2)
    ```
    ~$ . ~/jota/dmojsite/bin/activate
    (dmojsite) ~$ sudo python3 ~/jota/site/manage.py runbridged
    ```

3. JOTA Site (e.g. http://localhost:8001/ or http://<외부IP>:8001) 에 잘 접속되는지 확인합니다.

## Setting up uWSGI
original: https://docs.dmoj.ca/#/site/installation?id=setting-up-uwsgi

## Setting up supervisord
original: https://docs.dmoj.ca/#/site/installation?id=setting-up-supervisord

## Setting up nginx
original: https://docs.dmoj.ca/#/site/installation?id=setting-up-nginx

## Configuration of event server
original: https://docs.dmoj.ca/#/site/installation?id=configuration-of-event-server
