# Apache Jena Fuseki on Singularity

This guide provides instructions on how to build and run a Singularity container using the `focal.def` definition file. The container includes an Apache Jena Fuseki server, configured with Java 22.

## Acces With Singularity

### Creating Singularity Access Token for Remote Building
1. Create singularity account https://cloud.sylabs.io
2. Go to access tokens from profile in top right. 
3. In the text bar enter an alias for your token and hit +Create Accces Token
4. Download a copy of this token and copy to the clipboard for easy use in the following steps.
5. in the command line type `singularity remote login`. Once the prompt appers to enter the access token, paste it into the terminal and hit enter. After a short while, there should be output in the commandline indicating the token has been verified along with its storage location. 

### Generate Singularity Key Pair
1. In the CLI use `singularity keys newpair`. This will generate a new key pair.
	**Note**: When asked to enter your e-mail ensure it is the same one attached to your Singularity account. Otherwise, you will not be properly verified when attempting to push your key.
2. Use the command `singularity keys list` to pull up a list of your keys. Identify the long string of characters (i.e. D87FE3AF5C1F063FCBCC9B02F812842B5EEE5934) and copy it. 
3. Next, use `singularity keys push <your key here>`. You should see a return output stating your public key was successfully pushed to the server.

## Building the Container

### CLI Quick Reference

```
  # singularity build --remote --force --sandbox --fakeroot focal.sif focal.def
  # sudo singularity shell --writable focal.sif/
  # $JENA_HOME/fuseki start
  # $JENA_HOME/fuseki status
  # curl -X GET http://localhost:3030/$/server
  # $JENA_HOME/fuseki stop
```

### Guide and Explanation

To build the Singularity container from the `focal.def` file, follow these steps:

1. Navigate to the singularity directory
2. Use the command `vim focal.def` to create and enter the necessary definition file for singularity to build an image.
   - **Note**: Use your prefered text editor. VIM is not required.
3. Copy and paste the contents of `focal.def` from this repo, into the the local copy of `focal.def` you just created and are currently editing.
4. Use the command `singularity build --remote --force --sandbox --fakeroot focal.sif focal.def` to build the container.
	- **Note**: This process can take up to several minutes to complete. 
5. To enter the container use `singularity shell --writable focal.sif` command. This "allows you to spawn a new shell within your container and interact with it as though it were a small virtual machine."
   - **Note**: One way to verify that you are inside a singularity container is by looking at the command prompt, which will display `Singularity>` or something similar to `root@DESKTOP-KE54U6:/usr/local/singularity#`.

6. While inside the container you can use `$JENA_HOME/fuseki start` to start up the apache-jena-fuseki server.

   - To verify the server is running correctly do the following: `$JENA_HOME/fuseki status` and the output should say if Fuseki is running and give its PID.

   - Additionally, you can use the curl command `curl -X GET http://localhost:3030/$/server` which will output information about the server in JSON format.
   - Another way to verify the server is to go to use the command `hostname -I`. In your chrome browser type in `http://<ip-address-here>:3030. The apache-jena-fuseki webpage should appear and can be interacted with. 
	- **Note**: If two IP addresses appear copy the one on the right. 

7. To stop the server use the command `$JENA_HOME/fuseki stop`
