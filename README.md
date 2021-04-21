# Description
Get data from USB-connected UPS into InfluxDB using Telegraf

Transforms `upsc` output like this:
```
battery.charge: 100
battery.charge.low: 10
battery.charge.warning: 20
battery.mfr.date: CPS
battery.runtime: 3133
battery.runtime.low: 300
battery.type: PbAcid
battery.voltage: 13.1
battery.voltage.nominal: 12
```
into InfluxDB Line Protocol like this: 
```
ups battery.charge=100,battery.charge.low=10,battery.charge.warning=20,battery.mfr.date="CPS",battery.runtime=2970,battery.runtime.low=300,battery.type="PbAcid",battery.voltage=13.1,battery.voltage.nominal=12
```

## Usage

Edit the script `string_measurements` variable to reflect your setup. Specifically, add any additional measurement names that your UPS provides to the `string_measurements` array (in sorted order) so they will be included in the output.

To see all the measurements your UPS provides, run this:

```
upsc YOUR_UPS_NAME_HERE 2>/dev/null | awk -F':' 'BEGIN {print "string_measurements=["} {print "\"" $1 "\", "} END {print "]"}' |tr -d '\n'
```

Call the script from `telegraf.conf` like this
```
[[inputs.exec]]

   commands = ["python /path/to/getUpsData.py <YOUR_UPS_NAME_HERE>"]
   timeout = "5s"
   data_format = "influx"
```

## Compatibility
Tested on:
* Cyberpower CP1000AVRLCDa
* CyberPower SL700U (`CyberPowerSL700U.py`)
* Dell UPS 1000T/1920T/1920R HV (`DellBadge-Eaton5PX.py`)
* MGE Pulsar 2200


If you're using this with a different UPS, please let me know so I can add it to the list

## Contributors

Thanks to the following for helping improve this repo.

* @openincident
* @mattster98
* @Graffics
