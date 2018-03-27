Quickly thrown together faucet. Currently set to give out 10 XTL a pop. Limiting is set to 3 per day.



## Running this
First make sure RPC Wallet is running and fully sync'd.

`pip3 install -r requirements.txt`
You'll need to edit a file called 'faucet.ini'. The file should look like this:
```ini
[uwsgi]
module = wsgi:application
socket = /tmp/uwsgi.sock
chmod-socket = 666
chdir  = {project_dir}
wsgi-file = {path_file_wsgi-file.py}
master = true
processes = 1
threads = 1
vacuum = true
die-on-term=true

# environment
env=RECAPTCHA_PUBLIC_KEY={KEY_FROM_GOOGLE}
env=RECAPTCHA_PRIVATE_KEY={KEY_FROM_GOOGLE}
env=SECRET_KEY={random_string}
env=WTF_CSRF_SECRET_KEY={random_string}
env=FAUCET_ADDR={address_to_deposit_to}

# logging
logger = /path/to/errlog.log
re-logger = /path/to/reqlog.log
```

After that, run 
```python
python3 -c 'from faucet import db;db.create_all()'
```
then `uwsgi --ini faucet.ini`. Make sure you have turtlecoind and simplewallet running.
I left in the google analytics because I couldn't find a way to add that at deployment. Enjoy :)
