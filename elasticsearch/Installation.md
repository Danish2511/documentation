# Installation and Setup of Elasticsearch

## Step 1: Installing Elasticsearch on Your Local Machine

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

## Step 2: Installing Postman

1. **Download and Install Postman**     
    Go to the official website and download Elasticsearch for your OS.  
    
    - [Postman Download Page](https://www.postman.com/downloads/)

3. **Using Postman**    
    Postman is a user-friendly API client that lets you interact with APIs. In Elasticsearch, you'll use it to test your CRUD operations (Create, Read, Update, Delete).
