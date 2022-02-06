############ Begin Rabbit MQ  Setup ############
# Need to install rabbitmq first 
# If you have chocolatey just type
choco install rabbitmq

# Otherwise use the tutorial at this location
https://www.rabbitmq.com/install-windows.html#configure

# Rabbitmq should be running once you install it but if not use these options to kick it off
# Run RabbitMQ (On Windows) run this file
C:\Program Files\RabbitMQ Server\rabbitmq_server-3.8.6\sbin\rabbitmq-server.bat

# Launch rabbitmq server
rabbitmq-server

# Or this location
C:\Users\u0803160\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\RabbitMQ Server
############ End Rabbit MQ  Setup ############

# create virtual env
python -m venv ./venv

# select interpreter
ctrl+shift+p > python:select interpreter > select the one that says ('venv':venv)

# trash cmd window and close vs code when you open again you should automatically be in the env 
# shown by (venv) at beginning of prompt. If not type this command
venv/Scripts/Activate

# It helps to upgrade pip first
python.exe -m pip install --upgrade pip

# Install all other libraries with
# It will likely error our and you may have to run a few packages manually
# The creators of flower and beat haven't really kept up with the celery updates
pip install -r requirements.txt

# After that you need to run this script to revert celery back to a version that flower is compatible with
# Don't run this if you have a celery terminal running. Kill it with ctrl+c 
pip install celery==4.4.7

# Django terminal (check for migrations that need to be applied)
python manage.py runserver

### Celery ### terminal
celery -A fakeshop worker --pool=solo -l info

### Flower ### terminal 
celery -A fakeshop flower
(If this errors out revert the celery version again)

### Beat ### terminal
celery -A fakeshop beat -l INFO --scheduler django_celery_beat.schedulers:DatabaseScheduler

# Once all the terminals are running you can access the flower dashboard via
http://localhost:5555

# You can see all the rest of your tasks etc. in the Django Admin terminal
http://localhost/Admin
create a user with 'python manage.py createsuperuser'