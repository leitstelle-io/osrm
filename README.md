1. Download osm.pbf
    ```
    http://download.geofabrik.de/europe/germany/bayern/oberbayern.html
    ```
2. Extract OSRM 
    ```
    docker run --name osrm-extract --rm -t -v "${PWD}/data:/data" ghcr.io/project-osrm/osrm-backend osrm-extract -p /opt/car.lua /data/oberbayern-latest.osm.pbf || echo "osrm-extract failed"
    ```
3. Partition OSRM
    ```
    docker run --name osrm-partition --rm -t -v "${PWD}/data:/data" ghcr.io/project-osrm/osrm-backend osrm-partition /data/oberbayern-latest.osrm || echo "osrm-partition failed"
    ```
4. Customize OSRM
    ```
    docker run --name osrm-customize --rm -t -v "${PWD}/data:/data" ghcr.io/project-osrm/osrm-backend osrm-customize /data/oberbayern-latest.osrm || echo "osrm-customize failed"
    ```
5. Start OSRM Server 
    ```
    docker run --name osrm-backend --rm -t -i -p 5000:5000 -v "${PWD}/data:/data" ghcr.io/project-osrm/osrm-backend osrm-routed --algorithm mld /data/oberbayern-latest.osrm
    ```
6. Start OSRM Frontend
    ```
    docker run --name osrm-frontend --rm -p 9966:9966 osrm/osrm-frontend
    ``` 