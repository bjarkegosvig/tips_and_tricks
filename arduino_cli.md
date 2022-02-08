# arduino-cli


Some example on how to use the arduino cli and create a config.yaml file

<https://github.com/arduino/arduino-cli/issues/192>

 

Some commands to use without the config file

## Install model01 package
```ps1
arduino-cli core update-index --additional-urls "<https://raw.githubusercontent.com/keyboardio/boardsmanager/master/package_keyboardio_index.json>"

.\arduino-cli.exe core install arduino:avr keyboardio:avr --additional-urls <https://raw.githubusercontent.com/keyboardio/boardsmanager/master/package_keyboardio_index.json>
```

## Compile the Model01 sketch
 
```ps1
.\arduino-cli.exe compile -b keyboardio:avr:model01 C:\git\private_repos\Model01-Firmware
```
 
## Flash the Model01 sketch

```ps1
.\arduino-cli.exe upload COM4 -b keyboardio:avr:model01 C:\git\private_repos\Model01-Firmware
```
 
## Compile and flash Flash the Model01 sketch

```ps1
.\arduino-cli.exe compile –u –p COM4 -b keyboardio:avr:model01 C:\git\private_repos\Model01-Firmware
```
 

 

 

 
