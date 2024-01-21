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

ENV FLASK_ENV=development

EXPOSE ${PORT}

CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0", "--port=4567"]


## Frontend Dockerfile

FROM node:14

WORKDIR /frontend-react

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]


## 10 Container Security Best Practices
1. Keep host & Docker updated to latest security patches
2. Docker daemon & containers should run in non-root user mode
3. Image vulnerability scanning
4. Trusting a private vs public image registry
5. No sesitive data in Docker file or images
6. Use secret management services to share secrets
7. read only file system and volume for Docker
8. Seperate databases for long term storage
9. Use DevSecOps practices while building application security
10. Ensure all code is tested for vulnerabilities for production use

### Dynamodb 
 # DynomoDB yaml
    - dynamodb-local:
    user: root
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
       - "8000:8000"
    volumes:
       - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
    
## Postgres
     - db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - '5432:5432'
    volumes:
      - db:/var/lib/postgresql/data

   volumes:
  db:
    driver: local

    
## CLI Set-up

# CREATE A TABLE:
  aws dynamodb create-table \
    --endpoint-url http://localhost:8000 \
    --table-name Music \
    --attribute-definitions \
        AttributeName=Artist,AttributeType=S \
        AttributeName=SongTitle,AttributeType=S \
    --key-schema AttributeName=Artist,KeyType=HASH AttributeName=SongTitle,KeyType=RANGE \
    --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1 \
    --table-class STANDARD
# CREATE AN ITEM:
  aws dynamodb put-item \
    --endpoint-url http://localhost:8000 \
    --table-name Music \
    --item \
        '{"Artist": {"S": "No One You Know"}, "SongTitle": {"S": "Call Me Today"}, "AlbumTitle": {"S": "Somewhat Famous"}}' \
    --return-consumed-capacity TOTAL
# LIST TABLES: 
  aws dynamodb list-tables --endpoint-url http://localhost:8000

# GET RECORDS:
  aws dynamodb scan --table-name cruddur_cruds --query "Items" --endpoint-url http://localhost:8000
