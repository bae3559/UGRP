웹서버 구축 및 데이터 전달 (feat. APM, MySQL, phpMyAdmin, PHP)
============================================================

살펴볼 예제는 윈도우가 설치된 PC에 웹서버를 간단하게 구축하여 운영하는 방법에 관한 내용이다. 
 __APM setup__ 을 통해 __MySQL 데이터베이스__ 를 구축 후 __phpMyAdmin을 활용해서 웹서버를 관리__ 하는 과정을 진행할 것이다. 

+추가적으로 windows 방화벽에서 포트를 개방 후 __MySQL에 데이터를 전송하는 php 파일__ 을 생성하여 데이터베이스에 데이터를 전송하는 단계를 살펴 볼 것이다.


PHP란? 
-----------------------------------------------------------
### PHP?
PHP는 서버 측에서 실행되는 프로그래밍 언어로 HTML을 프로그래밍적으로 생성해주고, 데이터베이스와 상호작용 하면서 데이터를 저장하고, 표현한다.
+ 주로 HTML 코드를 프로그래밍적으로 생성 
+ 서버쪽에서 실행 되는 프로그래밍 언어!
+ Personal Home Page Tools의 약자 에서 PHP = Hypertext Preprocessor로 의미가 변경 되었다. 

## php 공부 할 수 있는 링크
[생활코딩] <https://opentutorials.org/course/62>   
[국내 최대의 PHP 커뮤니티] <https://phpschool.com>


-------------------------------------------------------------------------------------------

본론으로 돌아와서 웹서버를 구축하는 전체적인 순서를 한 번 살펴보고 넘어가도록 하자. 
```
1. APM setup 설치
2. phpMyAdmin 로그인 및 패스워드 재설정
3. phpMyAdmin 에서 데이터베이스 및 테이블 생성
4. 데이터 전송 및 외부 접속을 위한 포트 개방
5. 웹에서 데이터베이스에 데이터 전송하기 
6. 외부 네트워크에서 phpMyAdmin에 접속하기 위한 설정하기
```

---------------------------------------------------------------------------------------------

### 1. APM setup 설치   

[APM setup 설치 링크](https://drive.google.com/file/d/0B9eALrnw2M_ibDBoUVA1NXlsQkE/view, "download link")

APM setup 설치는 파일을 실행한 후 특별한 설정 없이 진행하면 된다.   
설치 후에는 웹상에서 "localhost"를 입력후에 다음 페이지가 나타나면 정상적으로 설치된 것이다. 
<img src="https://user-images.githubusercontent.com/42258047/86437178-ea9cfb80-bd3e-11ea-8895-502e69e6221e.PNG" ></img>

### 2. phpMyAdmin 로그인 및 패스워드 재설정


APM setup이 정상적으로 설치된 후에는 phpMyAdmin에 접속하여 로그인 후에 기본 패스워드 변경을 진행한다.   
웹상에서 ```localhost/myadmin/index.php``` 를 입력하면 아래와 같이 phpMyAdmin 로그인 페이지가 나타난다. 

<img src="https://user-images.githubusercontent.com/42258047/86437624-d7d6f680-bd3f-11ea-825a-d997ff7cbb4b.PNG" ></img>

```
기본 Username, Password
> Username : root
> Password : apmsetup
```

로그인 후 암호 변경이라는 버튼을 누른 후 변경하고 실행 버튼을 누르면 암호 변경이 완료된다. 

### 3. phpMyAdmin에서 데이터베이스 및 테이블 생성하기 

홈 화면에서 MySQL localhost 항목에 있는 __새 데이터베이스 만들기__ 아래 칸에 새로 만들 데이터베이스 명칭을 입력하고 만들기 버튼을 누르면 데이터베이스가 생성된다. 

<img src=""></img>

데이터베이스를 생성한 후에는 해당 데이터베이스 안에서 __테이블을 생성__ 한다. 테이블 명칭과 필드의 수를 입력 후 실행 버튼을 눌러준다.   

예를 들어 데이터의 형태를   
" 이름 , 학번, 키 " 이런 세트로 하고 싶으면 3개의 필드를 형성하면 되는 것이다. 테이블 생성 이후에도 ```Add```를 통해 필드를 추가할 수 있으니 뭐 고민하지 않아도 괜찮다! 

그리고 필요한 값들을 다 작성 완료 후 저장 버튼을 클릭해서 테이블을 생성해준다.

### 4. 데이터 전송 및 외부 접속을 위한 포트 개방

지금 위에 단계까지는 이제 phpMyAdmin에서 데이터베이스 및 테이블 생성이 완료되면 웹서버 구축은 완료가 된 상태이다.   
이번 단계에서는 외부에서 데이터를 전송하거나 phpMyAdmin에 접속하기 위해 포트를 개방하는 과정을 진행한다.    
현재 설치된 APM setup 의 기본 설정 포트인 80번 포트를 개방하여 해당 포트로 접속할 수 있도록 설정해주어야한다.   
__포트의 개방은 Windows 방화벽에서 가능하다__
``` > 제어판 > Windows방화벽 > 고급 설정 > 고급 보안이 포함된 Windows 방화벽 > 인바운드 규칙 > 새규칙 > 새 인바운드 규칙 마법사 > 
포트 선택 후 다음 > TCP, 특정 로컬 포트:80 설정 후 다음 > 연결 허용 선택 > 도메인, 개인, 공용 다 체크 후 다음 > 이름 : MySQL 이후 마침 > 
```

### 5. 웹에서 데이터베이스에 데이터 전송하기 

현재까지 phpMyAdmin 설치에서부터 Windows 방화벽 포트 개방까지 완료한 것이다.   
이번 단계에서는 외부에서 웹 서버에 데이터를 전송하기 위한 __php 파일__ 을 생성하고 생성한 php 파일을 활용하여 서버로 데이터를 전송해볼 것이다.   
   
앞선 단계에서 설치를 진행한 APM setup의 설치 폴더로 이동하면 ```htdocs``` 의 명칭을 가지는 폴더가 존재한다.   
해당 폴더를 열어보면 기본적으로 ```index.php``` 가 존재하며 이곳에 데이터를 전송하는 기능을 수행할 php 파일을 생성하도록 한다. 

생성할 php 파일의 코드는 다음과 같으며 해당 코드는 앞서 설정된 데이터 베이스 및 테이블에 데이터를 전송하기 위한 코드이므로 설정한 데이터베이스 환경에 맞추어 적절히 변경해서 사용하면 된다. 

```
<?php
header("Content-Type: text/html; charset=UTF-8");

$db_host="localhost";   
$db_user="root";
$db_passwd="apmsetup"; 
$db_name="TestDatabase";

$name = $_REQUEST["name"];
$time = $_REQUEST["time"];

echo $name;
echo $time;

$conn = mysqli_connect($db_host, $db_user, $db_passwd, $db_name);
mysqli_query($conn, "set session character_set_connection=utf8;");
mysqli_query($conn, "set session character_set_results=utf8;");
mysqli_query($conn, "set session character_set_client=utf8;");
$query = "INSERT INTO prototype (name, time) VALUES ('$name', '$time');";
$result = mysqli_query($conn, $query);

if($result)
      echo " |Result: OK";
    else
      echo " |Result: Error";

mysqli_close($conn);
?>
```

### 6. 외부 네트워크에서 phpMyAdmin 에 접속하기 위한 설정하기

현재 설정으로는 phpMyAdmin이 설치된 PC에서만 접속 및 관리가 가능하다.   
우리는 최종적으로 핸드폰 어플과 데이터를 주고 받을 것이므로 외부에서도 접속해서 관리를 할 수 있어야한다.    
따라서 외부 네트워크에서 접속할 수 있도록 추가 설정 과정이 필요하다.   
외부 접속을 위해 설정을 변경해줄 파일은 httpd-alias.conf 파일이다. 앞선 단계에서 설치를 진행한 APM setup 의 설치 폴더에서 아래의 순서로 진입하게 되면 해당 폴더 내에서 httdp-alias.conf 파일을 찾을 수 있을 것이다.

APM 이 설치된 경로 ```/APM_Setup/Server/Apache/conf/extra/```
httpd-alias.conf 파일을 열면 외부 접속을 가능하게 하려면 어떻게 변경해야하는지 주석처리가 되어있다. 그래서 주석내용과 동일하게 기존의 코드를 바꿔주면 됩니다.

윈도우를 재시작해도 무관하지만 서비스만 재시작하고 싶은 경우는 아래와 같이 진행한다. 

>> 윈도우 버튼 클릭 >> 프로그램 및 파일 검색에서 "services.msc"를 입력 후 엔터 >> 나타난 서비스 창에서 "APM_APACHE2"를 클릭한 후 다시 시작 클릭 

서버가 재시작된 후에 앞서 접속했던 주소("http://ip주소/myadmin/")로 다시 접속하면 아래와 같이 phpMyAdmin 페이지가 나타나는 것을 확인할 수 있습니다. 주소 입력 시에 myadmin 뒤에 "/"를 꼭 붙여주어야 한다!
