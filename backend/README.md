## 시작하기

1. 패키지 설치

   ```bash
   # FaceOFF/backend 디렉토리
   $ pip install torch==1.8.1+cpu torchvision==0.9.1+cpu torchaudio===0.8.1 -f https://download.pytorch.org/whl/torch_stable.html
   $ pip intall -r requirements.txt
   ```

   - 오류 발생시
     1. cmake 관련
        - 오류 메시지
          You must use Visual Studio to build a python extension on windows.  If you are getting this error it means you have not installed Visual C++.  Note that there are many flavors of Visual Studio, like Visual Studio for C# development.  You need to install Visual Studio for C++.
        - 해결방법
          Visual Studio의 C++ 을 선택하여 설치합니다.
     2. UnicodeDecodeError
        - 오류 메시지
          UnicodeDecodeError: 'utf-8' codec can't decode byte 0xbf in position 14: invalid start byte
        - 해결방법
          https://godpeople.or.kr/board/3361722 를 참고하여 pip/compat/__init__.py 파일을 수정합니다

2. migrate

   ```bash
   $ py manage.py makemigrations
   $ py manage.py migrate
   ```

3. 서버 실행

   ```bash
   $ py manage.py runserver
   ```

   

1. mysql 대신 sqlite3


# models.py 를 이용해 mysqlworkbench 에 table 생성하는법 

출처: https://django-document-korean.readthedocs.io/ko/master/intro/tutorial02.html 

## models.py에 DB 테이블로 넣을 class를 생성한다. 
ex) class User(models.Model):
    uid = models.UUIDField(db_column='UID', primary_key=True, default=uuid.uuid4, editable=False)  # Field name made lowercase.
    name = models.CharField(db_column='NAME', max_length=45, blank=True, null=True)  # Field name made lowercase.
    email = models.CharField(db_column='EMAIL', max_length=45, blank=True, null=True)  # Field name made lowercase.
    img = models.CharField(db_column='IMG', max_length=100, blank=True, null=True)  # Field name made lowercase.

    class Meta:
    #   managed =false -> 이 줄을 지워줘야 django 가 생성할 수 있다.
        db_table = 'USER'

## INSTALLED_APPS 에 user.apps.UserConfig 를 등록한다. 
-> 이제 django 는 user app 이 포함된 것을 알게되었다.
ex)   INSTALLED_APPS = [
    ...,
 'user.apps.UserConfig',
]

## migrations 를 만든다. (변경사항을 migration 으로 저장시키고 싶다고 django 에게 알림)
python manage.py makemigrations user

-> (성공시 0001로 숫자가 뜰것이다. 다음단계에서 사용한다. )

## sqlmigrate 를 이용해 migration 내용을 sql 로 변환한다. 
python manage.py sqlmigrate user 0001

## migrations 에 따라 필요한 데이터베이스 테이블을 생성한다. 
python manage.py migrate 

