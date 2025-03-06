Zero-downtime deployment using the blue-green strategy with Docker ensures that users experience no service interruptions while rolling out new versions of an application. Here’s how you can achieve this:

1. Understanding Blue-Green Deployment
Blue: The currently running version of the application.
Green: The new version of the application to be deployed.
The traffic is switched from the blue instance to the green instance seamlessly.


2. Steps for Zero-Downtime Deployment with Docker
Step 1: Run the Blue (Current) Version
Start the blue version of your application:




Step 2: Deploy the Green (New) Version
Run the new version (green) on a different port:



Step 3: Use a Reverse Proxy (NGINX)
Configure NGINX or Traefik to route traffic. Example NGINX config:

Step 4: Switch Traffic from Blue to Green

Step 5: Remove the Old (Blue) Version
Once traffic is successfully running on the green instance, remove the blue version:


Automating with Docker Compose
git status
git add .
git commit -m "live update "
git push origin main
-----------------------


1st step : Please install nginx as below details .
sudo apt install nginx -y 

sudo systemctl start nginx
sudo systemctl enable nginx
sudo nginx -s reload

Go nginx configure file and make bluegreen.conf file 

vi /etc/nginx/conf.d/bluegreen.conf

	 upstream flask_app {
		
			server localhost:5000;  # Placeholder (thisgets replaced dynamically)
			}
			
	server {
			listen 80; # make sure its on port 80 not 5001
			location / {
				proxy_pass http://flask_app; # This point is localhost: or 5000
				proxy_set_header Host $host;
				proxy_set_header X-Real-IP $remote_addr;
			}
}			

----------------------------------

Now Go github page on folder **Zero-Down-time-Blue-Green** on setting option .
on go secrets and variable option .
Go Action and create your DOCKER_USER,DOCKER_PASSWORD,AWS_HOST,AWS_USER,AWS_KEY 

Next Step ::  NOW Go your Git bash , edit index file and push . 

Database image will not change but app image will create blue and green . 


root@ip-172-31-31-235:~# docker ps
CONTAINER ID   IMAGE                           COMMAND                  CREATED         STATUS         PORTS                                                    NAMES
5943fd396d4c   ashis103/flask-attendance-app   "python run.py"          8 minutes ago   Up 8 minutes   0.0.0.0:5000->5000/tcp, [::]:5000->5000/tcp              flask-app-blue
a774dd35e167   mysql:latest                    "docker-entrypoint.s…"   8 minutes ago   Up 8 minutes   0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp, 33060/tcp   mysql-db
root@ip-172-31-31-235:~#
