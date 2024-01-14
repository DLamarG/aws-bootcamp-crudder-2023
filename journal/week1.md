# Week 1 — App Containerization

## Containerized Backend

## Run Python 
```sh
cd backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567
cd ..
```

- make sure to unlock the port on the port tab
- open link for 4567 port in your browser
- change url to "/api/activities/home"
- you should get back json


## Run Container
docker run --rm -p 4567:4567 -it backend-flask -e FRONTEND_URL='*' -e BACK
END_URL='*' backend-flask


## Backend Dockerfile
FROM python:3.11-slim-buster

WORKDIR /backend-flask

COPY requirements.txt requirements.txt

Run pip install -r requirements.txt

COPY . .

#Inside container and will remain set when the container is running
ENV FLASK_ENV=development

EXPOSE ${PORT}

CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0", "--port=4567"]
