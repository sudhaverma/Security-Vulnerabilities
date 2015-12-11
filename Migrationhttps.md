##HTTP to HTTPs Migration

**(Note: this is a work in progress. More details being added!.)**

With http, the content being sent across the network is not encrypted. Since the internet has become so pervasive, there was an urgent need to 
make sure any content is being sent in a secure manner. Thus, https is now used by default for almost all applications that transmit data over 
a network. 

Moving a running service from HTTP to HTTPS involves creating a certificate keystore and editing the Tomcat configuration file. Below, we go through the steps involved:

##What is SSL:
SSL, or [Secure Socket Layer](https://www.digicert.com/ssl.htm), is a technology which allows web browsers and web servers to communicate over a secured connection. 
This means that the data being sent is encrypted by one side, transmitted, then decrypted by the other side before processing.
This is a two-way process, meaning that both the server and the browser encrypt all traffic before sending out data.

### Steps for enabling https connection for a web application

*  Create Keystore in a directory

*  Generate Certificate Request in the keystore

*  Share the above Certificate Request with Windows-ADS Team for Certificate Generation

*  Once Certificate is received, also verify if it contains Chain Certificate

*  Export all Certificates individually to lowest format D64

*  FTP them to the directory containing keystore

*  Import the root chain certificate into the keystore

*  Import the intermediate chain certificate into the keystore

*  Import your certificate into the keystore

*  Make changes in the server.xml file of Tomcat and make the URL point to https instead of http and also direct it to this keystore

*  Check if the keystore is reflecting the latest certificate timestamp.
