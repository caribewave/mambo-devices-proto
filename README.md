# HAND sensors

This repository holds protobuf descriptors for the sensors used by the HAND teams.  

## Generate boilerplate code

Python:  
```docker run --rm -v $(pwd):$(pwd) -w $(pwd) znly/protoc --python_out=. -I. weather-station.proto```

Javascript:  
TODO


## Weather station
The weather stations (David Vantage VUE) stream data to a home-developed microchip.
This data is then encoded into protobuf frames and sent to the HAND-Cloud API.

Multiple packets exist in the protocol:

### WeatherStationDataPacket
This packet is emitted as frequently as possible when device connectivity is established.
The frame contains all live sensor-data of the station.

### WeatherStationAlarmPacket
This packet is emitted as soon as an alarm state changes.
The frame contains all alarm states.

### WeatherStationHiLowDataPacket
This packet contains all hi-low stats gathered from the weather station.

### WeatherStationStartSyncRequestPacket
This packet is sent to cloud to ask for a new synchronization. Will return the sync_id

### WeatherStationStartSyncResponsePacket
This packet is sent by cloud and contains the new synchronization id when a device attempts to perform a sync.

### WeatherStationSyncBatchPacket
This packet contains synchronization data for a specific sync_id. It is identified by a batch_id which will later be acknowledged (or not) by the broker.

### WeatherStationSyncIdPacket
This packet contains the sync_id.
It can either be used: 
 * to ask for the actual synchronization status
 * to ask for sync commit
 * to rollback a sync

### WeatherStationSyncStatusResponsePacket
This packet is sent by the broker in order to give sync status.
