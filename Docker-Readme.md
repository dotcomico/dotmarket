Docker Quick Instructions
🧱 Build the Image
Build all images defined in docker-compose.yml:
docker-compose build

▶️ Start the Container
Run the services:
docker-compose up

Run in the background:
docker-compose up -d

🔄 Update After Code or Dockerfile Changes
Rebuild and restart:
docker-compose up --build -d

🛑 Stop Containers
Stop running services:
docker-compose down

🧭 Enter a Running Container
Replace <service_name> with the name from your docker-compose.yml:
docker-compose exec <service_name> sh

Or, if the container uses bash:
docker-compose exec <service_name> bash

📋 Check Running Containers
docker ps




