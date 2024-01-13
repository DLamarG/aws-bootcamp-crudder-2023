FROM python:3.11-slim-buster
# Inside Cantainer
# make a new folder inside the container
WORKDIR /backend-flask

# Outside Container -> Inside Container
# this contains the libraries we want to install to run the app
COPY requirements.txt requirements.txt

# Inside Container
# Install the python libraries used for the app
Run pip install -r requirements.txt
# Outside Container -> Inside Container
# . means everything in the current directory
# first period . -- /backend-flask (outside cotainer)
# second period . -- /backend-lfash (inside container)
COPY . .

# Set Environment Variables (Env Vars)
#Inside container and will remain set when the container is running
ENV FLASK_ENV=development

EXPOSE ${PORT}

# CMD (Command)
# python3 -m flask run --host=0.0.0.0 --port=4567
CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0", "--port=4567"]
