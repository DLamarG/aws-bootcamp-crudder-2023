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
