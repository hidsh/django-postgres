# django-postgres

https://qiita.com/shigechiyo/items/9b5a03ceead6e5ec87ec

## memo

### venv お約束
```
❯❯❯ mkdir tuto
❯❯❯ cd tuto
❯❯❯ python -m venv venv
❯❯❯ . venv/bin/activate
(venv) ❯❯❯ python -m pip install --upgrade pip
```

### djanogo インストール
```
(venv) ❯❯❯ python -m pip install Django
(venv) ❯❯❯ python -m django --version
3.2.8
```

### psycopg2 インストール

```
(venv) ❯❯❯ pip install 'psycopg2-binary>=2.8,<2.9'
(venv) ❯❯❯ pip list |grep psycopg2
psycopg2-binary 2.8.6
```

### PostgreSQL インストール

#### Heroku に合わせて 13 を入れる
##### Ubuntu 20.04

[How To Install PostgreSQL 13 on Ubuntu 20.04 | 18.04](https://computingforgeeks.com/how-to-install-postgresql-13-on-ubuntu/)

##### Mac Big Sur
```
❯❯❯ brew install postgresql@13
❯❯❯ echo 'export PATH="/usr/local/opt/postgresql@13/bin:$PATH"' >> ~/.zshrc
❯❯❯ echo 'export LANNG=en_US.utf-8' >> ~/.zshrc
❯❯❯ echo 'export LC_ALL=en_US.utf-8' >> ~/.zshrc
❯❯❯ . ~/.zshrc
❯❯❯ psql -V
psql (PostgreSQL) 13.5
❯❯❯ brew services start postgresql@13
❯❯❯ brew services list|grep postgres
postgresql@13     started root
```

### PostgreSQL のユーザ作成

djangoから使うユーザ postgres を作成する。
まず、そののまま入る。
```
❯❯❯ psql postgres
postgres=# create user postgres SUPERUSER;
```

一旦抜けて postgresユーザで入り、個々のデータベース設定を確認する。
```
❯❯❯ psql -U postgres postgres
\l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | xxx      | UTF8     | C           | C           |
 template0 | xxx      | UTF8     | C           | C           | =c/xxx               +
           |          |          |             |             | g=CTc/xxx
 template1 | xxx      | UTF8     | C           | C           | =c/xxx               +
           |          |          |             |             | g=CTc/xxx
```

もし `template1` データベースの Collate と Ctype が上のように C になっている場合は新しく作るデータベースの日本語ソートがおかしくなるので、次のリンクのように `template1` を作り直しておく。→ https://yoshinorin.net/2018/06/12/postgresql-default-collate-ctype/

### PostgreSQL でアプリ用のDBを作成
```
psql -U postgres
 
create database myapp owner=postgres;
alter role postgres set client_encoding to 'utf8';
alter role postgres set default_transaction_isolation to 'read committed';
alter role postgres set timezone to 'Asia/Tokyo';
grant all privileges on database myapp to postgres;
\l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
 myapp     | postgres | UTF8     | ja_JP.UTF-8 | ja_JP.UTF-8 | =Tc/postgres         +
           |          |          |             |             | postgres=CTc/postgres
```

DB `myapp` に接続できることを確認
```
psql -U postgres -d myapp
```

### プロジェクト/アプリ作成
```
django-admin startproject mysite <-- プロジェクト作成
cd mysite                        <-- プロジェクトトップに移動(以後のコマンドは基本ここで実行)
python manage.py startapp myapp  <-- アプリ作成
```

### 開発用サーバ起動
```
python manage.py runserver
```

### Django のDB設定を変更
```
vim mysite/setting.py
```

以下のように変更。
```
 DATABASES = {                                                                            'default': {
         'ENGINE': 'django.db.backends.sqlite3',
         'NAME': BASE_DIR / 'db.sqlite3',
     }
 }

↓↓↓

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'myapp',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```

マイグレーションする。
```
python manage.py makemigrations
python manage.py migrate
```


### アプリのビューを追加
|ファイル|内容|メモ|
|---|---|---|
| polls/views.py | pollsアプリのビュー | リクエストハンドラ関数を書く|
| polls/urls.py  | pollsアプリ内のroute定義 | リクエストハンドラを追加したらこっちにも追加 |
| mysite/urls.py | プロジェクト内のroute定義| polls/url.py をimportする |

### 
```
```


## environment

### local (macbook pro / Big Sur 11.6.1)
- python 3.9.7
- pip 21.3.1
- django 3.2.9

### local (windows 10 / wsl2 / Ubuntu 20.04)
- python 3.8.10
- pip 21.3.1
- django 3.2.8 
- PostgreSQL 13.4
- psycopg2-binary-2.8.6

### heroku
- 


