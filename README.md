XMLTV grabber for xml EPG file
===================================

### Info
Parse, filter and change (by regex) an EPG xml file.
For more info and use: ```tv_grab_xml -h```

### CLI example 
* First start, configure the grabber  
```tv_grab_xml --configure --xml-source http://www.epg-guide.com/it.gz --config-file config.json```  
* Execute the grabber, specifying xmltv source and config file  
```tv_grab_xml --xml-source /path/to/epg.xml --config-file config.json```
* Execute the grabber with default config file  
```tv_grab_xml```

### Script flow

XMLTV (EPG xml) => tv_grab_xml (link by channels keys in CONFIG_FILE) => apply filter and changes rules => OUTPUT (stdout or file)

### CONFIG_FILE json example
```
"Rete 4":{
      "hidden":false,
      "replace":[
         {
            "xpath":".",
            "attrib":"id",
            "with":"Rete4"
         },
         {
            "xpath":"./icon[@src]",
            "attrib":"src",
            "with":"http://www.thelogodb.com/images/media/logo/rrrxrw1482080284.png"
         }
      ]
   }
```
* `Rete 4`: key must match with channel/id in XML_SOURCE
* `hidden`: display channel
* `replace`: list of tag (element) text/attributes to replace
* `{"xpath":".", "attrib":"id", "with":"Rete4"}`: replace channel/id with 'Rete4' to allow bind on TvHeadend
* `{"xpath":"./icon[@src]","attrib":"src","with":"http://www.thelogodb.com/images/media/logo/rrrxrw1482080284.png"}`: replace channel icon src