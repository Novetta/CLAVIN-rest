CLAVIN Rest
===========

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

We have updated the CLAVIN REST server to use Spring Boot. The port and HTTP methods remain unchanged, but we did update the API version.  So whereas prior you would go to http://localhost:9090/api/v0/geotag, you will now go to http://localhost:9090/api/v1/geotag. The application now has a application.yml file that contains the available settings.  Here are the instructions for getting up and running.  

### Download the CLAVIN Rest Project

    git clone https://github.com/Novetta/CLAVIN-rest.git
    cd CLAVIN-rest
	
### Download Geonames 
  
    curl -O http://download.geonames.org/export/dump/allCountries.zip

### Unzip Geonames 

    unzip allCountries.zip

### Inspect the application.yml

Look at (and edit if you so choose) the default configuration settings contained within the application.yml.

### Build the ClAVIN Rest jar

	mvn clean package

### Create a CLAVIN gazetteer 
    
    java -Xmx4096m -jar -Dspring.profiles.active=Build target/clavin-rest.jar

### Run the CLAVIN Rest server 

    java -Xmx2048m -jar target/clavin-rest.jar 

### Geotag an article  

From within the clavin-rest project, run the following curl command to retrieve all relevant geonames data for each location:

	curl -s --data @src/test/resources/Somalia-doc.txt --header "Content-Type: text/plain" http://localhost:9090/api/v1/geotag

From within the clavin-rest project, run the following curl command to retrieve minimized geonames data for each location:	

	curl -s --data @src/test/resources/Somalia-doc.txt --header "Content-Type: text/plain" http://localhost:9090/api/v1/geotagmin	

###	CLAVIN Rest web page

You can also see a very rudimentary web page by going to http://localhost:9090.  Copy and paste the Somalia-doc.txt file contents into the text area, and hit submit.  You'll wait for a few seconds (there is no loader to let you know its working), and voila! The map will be populated, and the resulting JSON will appear below the map. 

### CLAVIN Rest docker image

To build a docker image, you'll need to first tar.gz the IndexDirectory that contains the indexed geonames gazetteer. 
	
	tar -czvf IndexDirectory.tar.gz IndexDirectory

With the IndexDirectory.tar.gz in the project root directory, you can simply run the following command to build the Docker container. 

	mvn dockerfile:build 

To run the image in the foreground, you would then simply issue the docker run command. Or add the -d flag to detach and run in the background.

	docker run -p 9090:9090 clavin-rest:latest
	
