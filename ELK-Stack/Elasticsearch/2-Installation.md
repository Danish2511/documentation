# Installation and Setup of Elasticsearch

## Installing Elasticsearch on Your Local Machine

### Installation on Windows

1. **Download Elasticsearch**    
      Go to the official website and download Elasticsearch for your OS.

   - [Elasticsearch Download Page](https://www.elastic.co/downloads/elasticsearch)

2. **Unzip and Install**

   - **Windows**: Unzip the folder and navigate to the `bin` directory. Run the `elasticsearch.bat` file to start the server.
   - **Linux**: Extract the file and navigate to the `bin` directory. Run `./elasticsearch` in the terminal.

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
### Instalation with Docker Compose

1. **Chapter 1: Prerequisites**     
   - Docker and Docker Compose
2. **Download and Install Docker**:
   - For Windows/macOS: [Download Docker Desktop](https://www.docker.com/products/docker-desktop).
   - For Linux: Use your package manager to install Docker and Docker Compose.

3. **Verify Installation**:
   ```bash
   docker --version
   docker-compose --version
   ```

---

## **Chapter 2: Prepare the File Structure**

### Create the Project Folder

Include the following file structure:
```plaintext
├── .env
├── docker-compose.yml
```

### Add Content to the `.env` File
[docker-compose.yaml](./kafka-ui.yaml) - Click here to view docker-compose file.

---

## **Chapter 3: Configure Docker Compose**

[docker-compose.yaml](./kafka-ui.yaml) - Click here to view docker-compose file.

---

## **Chapter 4: Start the Containers**

1. Navigate to the directory containing `docker-compose.yml`.
2. Run the container:

   ```bash
   docker-compose up -d
   ```
---

## **Chapter 5: Verify the Installation**

1. Open a browser and navigate to:
   - Elasticsearch: `https://localhost:9200`
   - Kibana: `http://localhost:5601`
2. Log in to Kibana using the credentials:
   - Username: `elastic`
   - Password: The `ELASTIC_PASSWORD` from `.env`.

---