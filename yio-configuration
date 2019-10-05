The configuration is stored in `config.json` on the boot partition of the SD card.

### Accessing the configuration
The configuration class has the following methods:

`config.read` gives a JSON object. You can also access specific keys within this object: `config.read.settings.darkmode` for example.

`config.write` needs a JSON object to write. Example: `config.write = newconfig;` Please note, that this has to be the full configuration. If you want to change part of a config, do the following:

      var tmp = config.read;
      tmp.settings.darmkmode = false;
      config.write = tmp;


`config.readConfig(<path to config file>)` reads the config file from a file.

`config.writeConfig()` saves the config to the config file.

From C++ side, you can access the same methods:
- include the header file:
`#include "config.h`
- call a function 
`Config::getInstance()->read();`
