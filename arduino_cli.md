# arduino-cli


Some example on how to use the arduino cli and create a config.yaml file

<https://github.com/arduino/arduino-cli/issues/192>

 

Some commands to use without the config file

## Install model01 package
```ps1
arduino-cli config init
arduino-cli config add board_manager.additional_urls https://raw.githubusercontent.com/keyboardio/arduino-kaleidoscope-master/main/package_kaleidoscope_master_index.json
arduino-cli core update-index
# This line might needs to be run in a admin powershell
arduino-cli.exe core install arduino:avr keyboardio:avr
```
## Upgrade model01 package
```ps1
# This line might needs to be run in a admin powershell
arduino-cli.exe core upgrade arduino:avr keyboardio:avr
```

## Compile the Model01 sketch
 
```ps1
arduino-cli.exe compile -b keyboardio:avr:model01 C:\git\private_repos\Model01-Firmware
```
 
## Flash the Model01 sketch

```ps1
arduino-cli.exe upload -p COM4 -b keyboardio:avr:model01 C:\git\private_repos\Model01-Firmware
```
 
## Compile and flash Flash the Model01 sketch

```ps1
arduino-cli.exe compile -b keyboardio:avr:model01 C:\git\private_repos\Model01-Firmware -u -p COM4
```
 

 

 

 
