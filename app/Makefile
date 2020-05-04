.PHONY : start deploy check test

PYTHON=python3
DB_HOSTNAME=db:5432

start:
	./wait-for-it.sh $(DB_HOSTNAME) --timeout=10 --strict -- $(PYTHON) manage.py runserver 0.0.0.0:8080

deploy:
	rm -f /etc/nginx/sites-enabled/default
	rm -f /etc/service/nginx/down
	$(PYTHON) manage.py collectstatic --noinput
	./wait-for-it.sh $(DB_HOSTNAME) --timeout=10 --strict -- gunicorn --daemon --workers 8 --bind unix:/home/app/app.sock app.wsgi
	/sbin/my_init

check:
	flake8 && echo OK
	yapf -r --diff . && echo OK

test:
	./wait-for-it.sh $(DB_HOSTNAME) --timeout=10 --strict -- $(PYTHON) manage.py migrate
	coverage run --source="." manage.py test && coverage report
