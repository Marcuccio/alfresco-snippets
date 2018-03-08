#ALFRESCO NOTES

### USEFULL tests or check
- Ricerca avanzata
- Versionamento

### Important for alfresco 5.1+
To compile is important add the following properties. See the [documentation!](http://docs.alfresco.com/5.1/tasks/alfresco-sdk-upgrading-alfresco-version-SDK-220-510-to-511.html)
> mvn package -Ddependency.surf.version=6.3

#### Maven archetype generator:
Hyphens are typically used in artifact IDs.
Don't use "it-tai" as prefix for artifact IDs.
> mvn archetype:generate -Dfilter=org.alfresco:

#### General tips:

- Check of custom types model `/alfresco/s/api/classes/<prefix>_<name>`

- If you apply a hot edit to the `share-config-custom.xml` you need to refresh all your webscripts from `\share\page\index`.

- Retrieve a repositoryID: `curl -s -u admin:alfresco "http://<hostname>:8080/alfresco/s/cmis" | grep repositoryId `

- CMIS endpoint `/alfresco/service/api/cmis`

- To enable surfbug: `/share/page/surfBugStatus`

- To enable debug log level for JS-webscript: in `tomcat/shared/classes/alfresco/extension/custom-log4j.properties` change

	log4j.logger.org.alfresco.repo.jscript=debug
	log4j.logger.org.alfresco.repo.jscript.ScriptLogger=debug

### Start/Stop alfresco:
> tomcat/bin/startup.sh && tail -f tomcat/logs/catalina.out
> tomcat/bin/shutdown.sh

### Opzioni JVM alfresco:
> tomcat/bin/setenv.bat

### Hot deploy:
1. From "share" browse to `Repository>Data dictionary>Models`
2. Set property `model active = TRUE`
3. check "alfresco/service/api/classes/<prefix>_<name>
4. Will not visible until will the `share-config-custom.xml` will not be modified

### How to deploy AMPs:
1. java -jar /bin/alfresco-mmt.jar list      tomcat\webapps\alfresco
2. java -jar /bin/alfresco-mmt.jar uninstall <moduleName> tomcat\webapps\alfresco
3. java -jar /bin/alfresco-mmt.jar install   amps\<moduleName>-repo.amp tomcat\webapps\alfresco -force -nobackup -verbose
4. java -jar /bin/alfresco-mmt.jar install   amps_share\<moduleName>-share.amp tomcat\webapps\share -force -nobackup -verbose

### Userfull bash command/tips:
- List of running process: `ps -ef | grep java ` 
- List of stopped process: `jobs `
- List  of commands you are allowed to do: `sudo su -l `
- Bring foreground process towards: `fg ` 
- `netstat -an | grep alfresco | grep java | grep 8080 `
- `less ` -> "shif g" end file
- `less ` -> Search in: `/wordtosearch` next match: `n` 
- `sudo su - <user>`
- `sudo -i `
- `host <ip>` show ip
- `grep -rI --exclude-dir=test --exclude-dir=target "<wordtosearch>" . `
- `find . -name "<wordtosearch>" `
- `chown <owner>:<group> <file>`
- `find . -name service.xml -exec du -h  \{\} \; | sort -k1 -h` trova file mostra il size e ordina; -k1 sta per prima colonna
- `du -sh <directory>`

### Usefull snippets

Header to set in order to show or dowload a document in a respose object in a Webscript. [IETF DOC](www.ietf.org/rfc/rfc2183.txt):

``` java

    res.addHeader("Content-Disposition", "inline; filename=" + fileName);
    res.addHeader("Content-Disposition", "attachment; filename=" + fileName);
```

Visualiza images in base64:

``` html

    <img src="data:${work.mimetype};base64,${work.fileBase64}" class="thumbnail"/>
```

``` java

    InputStream inputStream = this.contentService.getReader( nodeRef , ContentModel.PROP_CONTENT ).getContentInputStream();
    byte[] arrBuff = IOUtils.toByteArray( inputStream );
    inputStream.close();
    return Base64.encodeBase64URLSafeString( arrBuff );
```

``` java
    fileFolderService.exists(nodeRef)
```

Check if node ref is valid and has right type:

``` java
    QName nodeType = nodeService.getType(dest);
    FileFolderServiceType fileFolderNodeType = fileFolderService.getType(nodeType);

    if (fileFolderService.exists(dest) || !fileFolderNodeType.equals(FileFolderServiceType.FOLDER) ) {
        throw new WebScriptException(
                HttpServletResponse.SC_BAD_REQUEST,
                "Destination folder must be of type "+ContentModel.TYPE_FOLDER);
    }
```
