# Setting JCloud Instance

*Last Update: 9. Jul. 2021*

**JCloud 가이드 레퍼런스**: https://jcloud-devops.github.io/user-guide.html

1. jcloud.jbnu.ac.kr 에 접속하여 로그인합니다.

2. Key Pairs 메뉴로 이동해서 Key Pair를 생성합니다. 이미 가지고 있는 경우 생략합니다.

3. Instances 메뉴로 이동합니다.

4. Launch Instance 버튼을 누릅니다.

5. 아래 명세를 만족하는 인스턴스 spec을 작성합니다.

    ```
    [Details]
      Instance Name = <your-instance-name>
      Description = <your-description>

    [Source]
      Select Boot Source = "Image"
      Create New Volume = "No"
      Available => Allocated = ["Ubuntu.20.04.ssh7777.VS-Code-Server"]

    [Flavor]
      Available => Allocated = ["cse-services.flavor"]

    [Networks]
      Available => Allocated = ["cse-students.network"]

    [Network Ports]   -- SKIP --
    [Security Groups] -- SKIP --

    [Key Pair]
      Available => Allocated = [<your-ssh-key>]

    [Configuration]   -- SKIP --
    [Server Groups]   -- SKIP --
    [Metadata]        -- SKIP --
    ```

6. Launch Instance 버튼을 누릅니다.

7. 페이지를 새로고침하여 인스턴스가 생성됐는지 확인합니다.

    | **Instance Name** | <your-instance-name>                |
    | ----------------- | ----------------------------------- |
    | **Image Name**    | Ubuntu.20.04.ssh7777.VS-Code-Server |
    | **IP Address**    | 10.0.ddd                            |
    | **Flavor**        | cse-services.flavor                 |
    | **Key Pair**      | **<your-ssh-key>**                  |
    | **Status**        | Active                              |
    | ...               | ...                                 |

8. 생성한 인스턴스에 접속합니다.

    - SSH 접속 방법
    - 만약 SSH접속 시 사용자이름을 입력해야한다면 ubuntu로 설정합니다.

    ```
    IP = 좌측 상단 JCloud 로고 [cse-students(xxx.xxx.xxx.xxx) ▼] 중 "xxx.xxx.xxx.xxx" 입력
    Port = 생성한 인스턴스의 사설 IP [10.0.0.ddd] 에서 "19ddd" 입력
    ```

    +++

    + Code-Server 접속 방법

    ```
    url: "http://xxx.xxx.xxx.xxx:10ddd"
    
    >> IP = 좌측 상단 JCloud 로고 [cse-students(xxx.xxx.xxx.xxx) ▼] 참조
    >> Port = 생성한 인스턴스의 사설 IP [10.0.0.ddd] 에서 "ddd" 참조
    >> Password = 생성한 인스턴스를 클릭하여 Log를 확인하여 상단의 Password 참조
    ```

    + Code-Server 접속 시 Password 변경 후 재부팅

    ```
    $ sudo vi /etc/code-server.conf
    $ sudo reboot
    ```

9. 유저 `ubuntu`의 비밀번호를 재설정합니다.

    ```
    $ sudo passwd ubuntu
    New password: <your-new-password>
    Retype new password: <your-new-password>
    passwd: password updated successfully
    ```

10. git 기록을 남길 본인의 이름과 이메일을 설정합니다.
    ```
    $ git config --global user.name "Gildong Hong"
    $ git config --global user.email "gildonghong@example.com"
    ```

    

