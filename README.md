# django-postgres

https://docs.djangoproject.com/ja/3.2/intro/tutorial01/

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
 pip install 'psycopg2-binary>=2.8,<2.9'
```

### PostgreSQL インストール

Heroku に合わせて 13 を入れる。以下参照。

[How To Install PostgreSQL 13 on Ubuntu 20.04 | 18.04](https://computingforgeeks.com/how-to-install-postgresql-13-on-ubuntu/)


### PostgreSQL でアプリ用のDBを作成
```
sudo -u postgres psql
 
create database myapp
alter role postgres set client_encoding to 'utf8';
alter role postgres set default_transaction_isolation to 'read committed';
alter role postgres set timezone to 'Asia/Tokyo';
grant all privileges on database myapp to postgres;
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

### local (macbook pro)
- python 3.9.7
- pip 21.3.1
- django 3.2.8 

### local (windows 10 / wsl2 / Ubuntu 20.04)
- python 3.8.10
- pip 21.3.1
- django 3.2.8 
- PostgreSQL 13.4
- psycopg2-binary-2.8.6

### heroku
- 


