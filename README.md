XMLTV grabber for xml EPG file
==============================

## Info

Parse, edit, filter and change (by regex) an EPG xml file. Compatible with the TVHeadend EPG grabber modules.
For more info and use: `tv_grab_xml -h`

## Install

1. `git clone git@github.com:elbowz/tv_grab_xml.git tv_grab_xml` (download)
2. `sudo ln -s /path/to/tv_grab_xml/tv_grab_xml /usr/bin` (global install, compatible with TVHeadend )
3. `tv_grab_xml --configure --xml-source http://www.epg-guide.com/it.gz` (first time configuration) 
4. `vim $HOME/.xmltv/tv_grab_xml.conf.json` (edit as you prefer the config file)
5. `tv_grab_xml`

## CLI example

* First start, configure the grabber. Read the xml source and create a base config file with all channels
```tv_grab_xml --configure --xml-source http://www.epg-guide.com/it.gz --config-file config.json```  
* Execute the grabber, specifying xmltv source (overriding the source in `config.json`) and config file  
```tv_grab_xml --xml-source /path/to/epg.xml --config-file config.json```
* Execute the grabber with default config file
```tv_grab_xml```

## Script flow

XMLTV (EPG xml) => tv_grab_xml (link by channels keys in CONFIG_FILE) => apply filter and changes rules => OUTPUT (stdout or file)

## Channel example

### XML_SOURCE

```xml
<channel id="Rete 4"><display-name lang="it">Rete 4</display-name></channel>
```

### CONFIG_FILE

```json
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
      ],
      "add":[
        {
          "xpath": ".",
          "xml": "<display-name lang=\"it\">Rete 4 HD</display-name>"
        }
      ]
   }
```

* `Rete 4`: key must match with `./channel[@id]` in XML_SOURCE
  * `hidden`: hide channel
  * `replace`: list of tags (elements) text/attributes to replace
    * `{"xpath":".", "attrib":"id", "with":"Rete4"}`: replace `./channel[@id]` with 'Rete4' to allow bind on TVHeadend
    * `{"xpath":"./icon[@src]","attrib":"src","with":"http://www.thelogodb.com/images/media/logo/rrrxrw1482080284.png"}`: replace channel icon src
  * `add`: list of tags (elements) to append
    * `{ "xpath": ".", "xml": "<display-name lang=\"it\">Rete 4 HD</display-name>"}` add/append to current channel (ie. `Rete 4`) the new `<display-name />` element as children. 

## Fields description (CONFIG_FILE)

* `hidden`: true/false print channel. True is as remove entire channel line. (default: false)'
* `replace`: list of tags (elements) text/attributes to replace
  * `xpath`: path to the 'channel' (sub)element where replace text/attrib value (eg. `./icon[@src]`). More info at: [https://docs.python.org/3.6/library/xml.etree.elementtree.html#elementtree-xpath]()
  * `attrib`: element attribute name to replace. If not set, replace the content of the element (optional)
  * `regex`: use regex groups (eg. `([a-z]*).jpg`) to reuse them in `with` field (optional)
  * `with`: string replacing the original. With regex, use '\1..\n' for groups
* `add`: list of tags (elements) to append
  * `xpath`: path to the 'channel' (sub)element where add/append the new xml (eg. `.` current). More info at: [https://docs.python.org/3.6/library/xml.etree.elementtree.html#elementtree-xpath]()
  * `attrib`: 'attribute to set. It works as in replace (optional)',
  * `xml`: raw xml to append to element target selected by xpath

## :sparkling_heart: Support the project

I open-source almost everything I can. If you are using this project and are happy with it, please consider one of these ways to support the project (and me):

* Giving proper credit when you use it, linking back to it :D
* Starring :star2: and sharing the project :rocket:
* Submit PRs, bug reports and feature requests
* [![Paypal][paypal-shield]][paypal-url] Make one-time donations via PayPal. I'll probably buy a :beer:

Thanks! :heart:

[paypal-shield]: https://img.shields.io/badge/-Donate-black.svg?style=flat-square&logo=paypal&colorB=555
[paypal-url]: https://www.paypal.me/EmanuelePalombo