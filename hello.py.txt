import os
stream = os.popen("echo \"$(($(sudo docker ps | wc -l) - 1))\"")
out = stream.read()

from flask import Flask
app = Flask(__name__)
@app.route('/metrics/')
def Counter():

   return "count_of_containers " + str(out)
