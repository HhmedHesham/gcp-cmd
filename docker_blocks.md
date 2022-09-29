for frontend 

FROM gcr.io/google_appengine/python
RUN virtualenv -p python3.7 /env
ENV VIRTUAL_ENV /env
ENV PATH /env/bin:$PATH
ADD requirements.txt /app/requirements.txt
RUN pip install -r /app/requirements.txt
ADD . /app
CMD gunicorn -b 0.0.0.0:$PORT quiz:app

for backend
FROM gcr.io/google_appengine/python
RUN virtualenv -p python3.7 /env
ENV VIRTUAL_ENV /env
ENV PATH /env/bin:$PATH
ADD requirements.txt /app/requirements.txt
RUN pip install -r /app/requirements.txt
ADD . /app
CMD python -m quiz.console.worker

/**********
Enter the Dockerfile command to initialize the creation of a custom Docker image using Google's Python App Engine image as the starting point.
Writes the Dockerfile commands to activate a virtual environment.
Writes the Dockerfile command to execute pip install as part of the build process.
Writes the Dockerfile command to add the contents of the current folder to the /app path in the container.
Completes the Dockerfile by entering the statement, gunicorn ..., that executes 
when the container runs. Gunicorn (Green Unicorn) is an HTTP server that supports the 
Python Web Server Gateway Interface (WSGI) specification
**********/

