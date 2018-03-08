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

#### Utils:

- Check di un tipo documentale `/alfresco/s/api/classes/<prefix>_<name>`

- Se modificato il `share-config-custom.xml` è necessario refreshare i webscript da `\share\page\index`.

- curl -s -u admin:alfresco "http://<hostname>:8080/alfresco/s/cmis" | grep repositoryId

- Comando JAVA con classpath espilicito e redirect dell'output `java -cp ../lib/* -jar *.jar --console --port 9979 > /dev/null 2>&1 &`

- Endpoint cmis da puntare `/alfresco/service/api/cmis`

- Abilitare surfbug: `/share/page/surfBugStatus`

- Abilitare il log per i webscript JS in `tomcat/shared/classes/alfresco/extension/custom-log4j.properties`

	log4j.logger.org.alfresco.repo.jscript=debug
	log4j.logger.org.alfresco.repo.jscript.ScriptLogger=debug

### Start alfresco:
> tomcat/bin/startup.sh|.bat

### Opzioni JVM alfresco:
> tomcat/bin/setenv.bat

### Deploy a caldo:
1. da share > Repository>Data dictionary>Models
2. set metadato "model active" = TRUE
3. check "alfresco/service/api/classes/<prefix>_<name>
4. non attivo in share sino a che non si sarà  modificato il share-config-custom.xml

### Installazione AMP:
1. /alfresco-one/bin
2. java -jar /bin/alfresco-mmt.jar list      tomcat\webapps\alfresco
3. java -jar /bin/alfresco-mmt.jar uninstall modulo1 tomcat\webapps\alfresco
4. java -jar /bin/alfresco-mmt.jar install   amps\modulo1-repo.amp tomcat\webapps\alfresco -force -nobackup -verbose
5. java -jar /bin/alfresco-mmt.jar install   amps_share\modulo1-share.amp tomcat\webapps\share -force -nobackup -verbose

### Utils VM:
- Path to scripts `/etc/init.d/`
- Path to alfresco `/opt/alfresco/`
- `ps -ef | grep java ` List running process
- `netstat -an | grep alfresco | grep java | grep 8080 `
- `less ` -> "shif g" end file
- `less ` -> "/<cosacercare>" "n prossimo match"
- `sudo su - <user>`
- `sudo -i `
- `sudo su -l ` lista di comandi che puoi fare
- `jobs `list stopped process
- `fg ` bring foreground process towards
- `host <ip>` show ip
- `grep -rI --exclude-dir=test --exclude-dir=target "cosa cercare" . `
- `find . -name "cosacercare" `
- `chown <owner>:<group> <file>`
- `find . -name service.xml -exec du -h  \{\} \; | sort -k1 -h` trova file mostra il size e ordina; -k1 sta per prima colonna
- `du -sh <directory>`

### Usefull snippets

Header per visualizzare o scaricare un documento . [IETF DOC](www.ietf.org/rfc/rfc2183.txt).

``` java

    res.addHeader("Content-Disposition", "inline; filename=" + fileName);
    res.addHeader("Content-Disposition", "attachment; filename=" + fileName);
```

Visualizzazione immagini in base64

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

Check if node ref is valid and has right type
``` java
    QName nodeType = nodeService.getType(dest);
    FileFolderServiceType fileFolderNodeType = fileFolderService.getType(nodeType);

    if (fileFolderService.exists(dest) || !fileFolderNodeType.equals(FileFolderServiceType.FOLDER) ) {
        throw new WebScriptException(
                HttpServletResponse.SC_BAD_REQUEST,
                "Destination folder must be of type "+ContentModel.TYPE_FOLDER);
    }
```