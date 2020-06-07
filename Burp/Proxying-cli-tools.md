# How to proxy CLI tools on Burp

## Proxy curl/wget
```
export http_proxy=localhost:8080
export https_proxy=localhost:8080
curl -k https://ifconfig.io
wget --no-check-certificates  https://ifconfig.io
```

## Proxy Java JARS
- Find JAVA_HOME: 
```
java -XshowSettings:properties -version 2>&1 > /dev/null | grep 'java.home'
```

- Import Burp CA - default PW: changeit
```
$JAVA_HOME/bin/keytool -import -alias burpsuite -keystore $JAVA_HOME/lib/security/cacerts -file $HOME/certs/burpca.crt -trustcacerts
```
- Proxy Java
```
java -Dhttp.nonProxyHosts= -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=8080 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=8080 -jar <yourjar>.jar
```

## Proxy Python requets
- Ensure you run the right Python interpreter
- Find CA Bundle with certifi:
```
python -c "import certifi; print(certifi.where())"
```
- Add Burp CA PEM to the end of file:
```
cat ~/certs/burpca.pem >> <pythonhome>/libexec/lib/python3.7/site-packages/certifi/cacert.pem
```

## Proxy Node JS/NPM Packages

- Download and install global-agent
```
mkdir nodeproxy && cd nodeproxy
npm install global-agent
```
- Export env variables and inject dependency
```
export NODE_EXTRA_CA_CERTS=$HOME/certs/burpca.crt
export GLOBAL_AGENT_HTTP_PROXY=http://127.0.0.1:8080
node -r 'global-agent/bootstrap' <path_to_module>
```

## Proxy Go binaries
- Install Burp CA into your OS trusted certificates
- Export env variable and run
```
https_proxy=127.0.0.1:8080 <gobinary>
```

## Links
- https://blog.ropnop.com/proxying-cli-tools/
