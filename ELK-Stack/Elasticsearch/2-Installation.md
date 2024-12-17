# Installation and Setup of Elasticsearch

## Step 1: Installing Elasticsearch on Your Local Machine

### Windows Installation

1. **Download Elasticsearch**  
    Go to the official website and download Elasticsearch for your OS.

   - [Elasticsearch Download Page](https://www.elastic.co/downloads/elasticsearch)

2. **Unzip and Install**

   - **Windows**: Unzip the folder and navigate to the `bin` directory. Run the `elasticsearch.bat` file to start the server.
   - **Linux or macOS**: Extract the file and navigate to the `bin` directory. Run `./elasticsearch` in the terminal.

3. **Verify Installation**  
   Open a browser and go to [http://localhost:9200](http://localhost:9200). You should see a response similar to this:

   ```json
   {
     "name": "your_node_name",
     "cluster_name": "elasticsearch",
     "cluster_uuid": "some_uuid",
     "version": {
       "number": "x.x.x",
       "build_flavor": "default",
       "build_type": "tar",
       "build_hash": "hash_here",
       "build_date": "date_here",
       "lucene_version": "lucene_version_here",
       "minimum_wire_compatibility_version": "some_version",
       "minimum_index_compatibility_version": "some_version"
     },
     "tagline": "You Know, for Search"
   }
   ```

### Installation using Docker

1. **Install Docker Desktop (If not)**  
    Go to the official website and download Docker Desktop.

   - [Docker Download Page](https://www.docker.com/products/docker-desktop/)

2. **Start Docker Desktop** and ensure it’s running. You can check the installation by opening a terminal or command prompt and running:

   ```bash
    docker --version
   ```

   If Docker is installed correctly, you’ll see the version information.

   - **Create a new docker network:** Once Docker is installed, open a terminal or command prompt and run:

   ```bash
    docker network create elastic
   ```

   - **Pull the Elasticsearch Docker Image:** Once Docker is installed, open a terminal or command prompt and run:

   ```bash
    docker pull docker.elastic.co/elasticsearch/elasticsearch:8.15.3
   ```

   - **Run an Elasticsearch Container:** Run Elasticsearch in a container using the following command:

   ```bash
    docker run --name elasticsearch --net elastic -p 9200:9200 -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.15.3
   ```

3. **Verify Installation**  
   Open a browser and go to [http://localhost:9200](http://localhost:9200). You should see a response similar to this:

   ```json
   {
     "name": "your_node_name",
     "cluster_name": "elasticsearch",
     "cluster_uuid": "some_uuid",
     "version": {
       "number": "x.x.x",
       "build_flavor": "default",
       "build_type": "tar",
       "build_hash": "hash_here",
       "build_date": "date_here",
       "lucene_version": "lucene_version_here",
       "minimum_wire_compatibility_version": "some_version",
       "minimum_index_compatibility_version": "some_version"
     },
     "tagline": "You Know, for Search"
   }
   ```

## Step 2: Installing Postman

1. **Download and Install Postman**  
   Go to the official website and download Elasticsearch for your OS.

   - [Postman Download Page](https://www.postman.com/downloads/)

2. **Using Postman**  
   Postman is a user-friendly API client that lets you interact with APIs. In Elasticsearch, you'll use it to test your CRUD operations (Create, Read, Update, Delete).
