# Apache Jena Fuseki on Singularity

This guide provides instructions on how to build and run a Singularity container using the `focal.def` definition file. The container includes an Apache Jena Fuseki server, configured with Java 22.

## Building the Container

### CLI Quick Reference

```
  # singularity build focal.sif focal.def
  # singularity shell focal.sif
  # $JENA_HOME/fuseki start
  # $JENA_HOME/fuseki status
  # curl -X GET http://localhost:3030/$/server
  # $JENA_HOME/fuseki stop
```

### Guide and Explanation

To build the Singularity container from the `focal.def` file, follow these steps (assuming you have sudo access):

1. Navigate to the singularity directory
2. Use the command `vim focal.def` to create and enter the necessary definition file for singularity to build an image.
   - **Note**: Use your prefered text editor. VIM is not required.
3. Copy and paste the contents of `focal.def` from this repo, into the the local copy of `focal.def` you just created and are currently editing.
4. Use the command `singularity build focal.sif focal.def` to build the container.
5. To enter the container use `singularity shell focal.sif` command. This "allows you to spawn a new shell within your container and interact with it as though it were a small virtual machine."

   - **Note**: One way to verify that you are inside a singularity container is by looking at the command prompt, which will display `Singularity>` or something similar to `root@DESKTOP-KE54U6:/usr/local/singularity#`.

6. While inside the container you can use `$JENA_HOME/fuseki start` to start up the apache-jena-fuseki server.

   - To verify the server is running correctly do the following: `$JENA_HOME/fuseki status` and the output should say if Fuseki is running and give its PID.

   - Additionally, you can use the curl command `curl -X GET http://localhost:3030/$/server` which will output information about the server in JSON format.
   - Another way to verify the server is to go to the URL `http://localhost:3030` and verify the Fuseki homescreen appears.

7. To stop the server use the command `$JENA_HOME/fuseki stop`
