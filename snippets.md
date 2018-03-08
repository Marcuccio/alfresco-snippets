# ALFRESCO NOTES 

#### Maven archetype generator:
> mvn archetype:generate -Dfilter=org.alfresco:

Hyphens are typically used in artifact IDs not in group IDs

#### General tips:

- To check out custom types model already deployed go to `/alfresco/s/api/classes/<prefix>_<name>`

- If you apply a hot edit to the `share-config-custom.xml` you need to refresh all your webscripts from `/share/page/index`.

- Retrieve a repositoryID: `curl -s -u admin:alfresco "http://<hostname>:8080/alfresco/s/cmis" | grep repositoryId `

- CMIS endpoint `/alfresco/service/api/cmis`

- To enable surfbug: `/share/page/surfBugStatus`

- To enable debug log level for JS-webscript: go to `tomcat/shared/classes/alfresco/extension/custom-log4j.properties` and change
````
	log4j.logger.org.alfresco.repo.jscript=debug
	log4j.logger.org.alfresco.repo.jscript.ScriptLogger=debug
````

### Start/Stop alfresco:
````
	tomcat/bin/startup.sh && tail -f tomcat/logs/catalina.out 
	tomcat/bin/shutdown.sh
````

### Opzioni JVM alfresco:
> tomcat/bin/setenv.bat

### Hot deploy:
1. In Alfresco-Share navigate to `Repository>Data dictionary>Models`
2. Set property `model active = TRUE`
3. check `alfresco/service/api/classes/<prefix>_<name>`
4. Will not visible until the `share-config-custom.xml` will not be modified

### How to deploy AMPs:
1. ` java -jar /bin/alfresco-mmt.jar list      tomcat\webapps\alfresco `
2. ` java -jar /bin/alfresco-mmt.jar uninstall <moduleName> tomcat\webapps\alfresco `
3. ` java -jar /bin/alfresco-mmt.jar install   amps\mymodule-repo.amp tomcat\webapps\alfresco -force -nobackup -verbose `
4. ` java -jar /bin/alfresco-mmt.jar install   amps_share\mymodule-share.amp tomcat\webapps\share -force -nobackup -verbose `

### Usefull bash command/tips:
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

Header to set in order to show or dowload a document in a respose object in a Webscript. [IETF DOC](https://www.ietf.org/rfc/rfc2183.txt):

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

Check if node ref is valid and has right type:

``` java
    fileFolderService.exists(nodeRef)
```

``` java
    QName nodeType = nodeService.getType(dest);
    FileFolderServiceType fileFolderNodeType = fileFolderService.getType(nodeType);

    if (fileFolderService.exists(dest) || !fileFolderNodeType.equals(FileFolderServiceType.FOLDER) ) {
        throw new WebScriptException(
                HttpServletResponse.SC_BAD_REQUEST,
                "Destination folder must be of type "+ContentModel.TYPE_FOLDER);
    }
```
