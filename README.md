
**dataminer** is a MTConnect utility to demonstrate how to collect data from MT agents and store them in a scalable data warehouse so they can be searched, analyzed, and visualized it in real time. It is written in C++ using boost and openssl libraries. 

In this demonstration, we use Open Source Elastic Search technology stack, **ELK**, to build a scalable data warehouse. ELK has three key components: 

[ElasticSearch](https://www.elastic.co/products/elasticsearch) is a distributed, search and analytics engine.

[LogStash](https://www.elastic.co/products/logstash) Logstash is a data processing pipeline that ingests data from various sources simultaneously, transforms it, and then sends it to ElasticSearch data engine.

[Kibana](https://www.elastic.co/products/kibana) visualizes the data from the ElasticSearch data engine. 

The following diagram depicts The interaction among MTConnect agents, dataminer and ELK components:

![dataminer_ELK](https://user-images.githubusercontent.com/34289248/61575977-5b90c800-aa88-11e9-8f52-06bd5d01215d.png)

Here is a screenshot from Kibana displaying XYZ positions of a device in real time:

![dataminer_Kibana](https://user-images.githubusercontent.com/34289248/61575981-6c413e00-aa88-11e9-8cc7-4c40bc06df43.png)

Also monitor alarms from the devices:

![Alarm Monitor](https://user-images.githubusercontent.com/34289248/75591517-c77eda00-5a34-11ea-83a5-d9e66464551c.png)

Building
-------

**dataminer** is written in C++ using **boost** and **openssl** libraries. It uses **CMake** as the build system. First download and install them:

- [CMake](https://cmake.org)
- [boost](https://www.boost.org)
- [openssl](https://www.openssl.org) - This is to support https secure protocol. For Windows, after the build, prepend the location of libcrypto.dll and libssl.dll to the PATH system variable.

Then run these commands:

- **cmake Makefiles.list**
- **cmake --build . --target all**

If build successful, dataminer should be generated in the current directory.

Usage:

- dataminer **[ json file output location ] [ MTConnect Agent URL address ]**

ELK Configuration
-------

1. Start ElasticSearch and Kibana service with the default configuration. 

2. Configure LogStash to digest the json files generated by dataminer. 

    First, update **path** variable in **MTDataMiner.conf** for the location of the output json files from dataminer. For example:

    **path =>** [ **"/tmp/MTComponentStreams-*.log"** ]

    Copy MTDataMiner.conf to logstash local config location like /usr/local/etc/logstash. Update/Add **path.config** in **logstash.yml** to use this MTDataMiner.conf.

    **path.config: /usr/local/etc/logstash/MTDataMiner.conf**

3. Start LogStash service.

4. Verify the setup, login to Kibana and look for index **mtconnect-demo**. If found, create an index pattern from **mtconnect-demo**. 

You can then start analyzing your data via ELK.

v1.1.0 Release Notes
-------
1. Output data items (sample, event or condition) to individual json records with their associated device/component id attributes.
2. Determine numeric data items by using info from "probe" request.
3. Poll data history using "sample" requests and track last request's sequence number so it can recover on restart. If last sequence number is not present, it starts from the snapshot of current data.
4. Support polling from multiple agents with different frequencies. 

v1.1.1 Release Notes
-------
1. Remove various MinGW DLLs dependencies like gcc, stdc++.
2. Remove prebuilt binaries from the repo. Move them to the release page.
3. Create prebuilt binaries for Ubuntu and Raspbian

Pre-built binaries
-------

MacOS, Ubuntu, Windows and Raspbian pre-built binaries are available in 
https://github.com/mtconnect/dataminer/releases
