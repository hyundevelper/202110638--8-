# DHT11 - 라즈베리파이 4 + Node-RED + DB

DHT11 센서 기반 온도 및 습도 데이터 수집 시스템 구축

# 1. 프로젝트 개요
이 프로젝트에서는 DHT11 센서를 통해 온도와 습도 데이터를 수집하고, Node-RED를 사용하여 실시간으로 시각화하며, 수집된 데이터를 MariaDB에 저장하는 시스템을 구축합니다.

# 2. 필요 장비
Raspberry Pi 4
DHT11 센서
브레드보드 및 점퍼 와이어
Node-RED (Raspberry Pi에 사전 설치되어 있어야 함)
MariaDB (데이터 저장을 위한 데이터베이스)

# 3. Node-RED 환경 설정
Node-RED에서 DHT11 센서를 활용한 데이터 수집 흐름을 설정합니다. 아래의 이미지와 같은 형태로 플로우를 구성합니다. (이미지는 사용자의 상황에 맞게 추가해 주세요.)

참고: 현재 DHT11 센서의 인식 오류로 인해 온도 및 습도 데이터가 표시되지 않지만, 기본 구조만 설정해 둡니다. 센서를 교체한 후 문제가 해결되면 보고서를 업데이트할 예정입니다.

# 4. MariaDB 설정 및 데이터베이스 구축
#4.1 MariaDB 설치

1. mariadb-server 패키지를 설치합니다.

sudo apt install mariadb-server


# 4.2 MySQL 서비스 시작
MariaDB 서비스를 시작합니다.

sudo systemctl start mariadb


# 4.3 사용자 설정
root 사용자로 로그인한 후, 새로운 사용자 계정을 생성하고 필요한 권한을 부여합니다.

sudo mysql -u root -p
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';

FLUSH PRIVILEGES;


# 4.4 테이블 생성 및 확인
raspi_dht11 데이터베이스를 생성한 후, collect_data_red라는 테이블을 만듭니다.


CREATE DATABASE raspi_dht11;
USE raspi_dht11;
CREATE TABLE collect_data_red (
    id INT AUTO_INCREMENT PRIMARY KEY,
    temperature FLOAT,
    humidity FLOAT,
    timestamp DATETIME DEFAULT CURRENT_TIMESTAMP
);
SHOW TABLES;


# 4.5 데이터 확인
생성한 테이블에 데이터가 저장될 수 있도록 Node-RED와 연동하여 데이터를 수집하고, 이후 SELECT 명령어를 통해 데이터베이스 내 데이터를 확인합니다.

SELECT * FROM collect_data_red;
