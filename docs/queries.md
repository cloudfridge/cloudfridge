List all paths of Cloudfridge devices:

`$ curl -XPOST -d 'select Path where Path like "/cloudfridge/%"' http://ard.cloudfridge.io:8079/api/query`

POST http://ard.cloudfridge.io:8079/api/query
select Path where Path like '/cloudfridge/%' 

    [
      {"Path": "/cloudfridge/T-001/evaporator_power"}, 
      {"Path": "/cloudfridge/T-002/compressor_power"}, 
      {"Path": "/cloudfridge/T-002/evaporator_power"}, 
      {"Path": "/cloudfridge/total_power"}, 
      {"Path": "/cloudfridge/T-003/compressor_power"}, 
      {"Path": "/cloudfridge/T-001/compressor_power"}, 
      {"Path": "/cloudfridge/Nebula-235a75b236a7c9ee/cabin_temperature"}, 
      {"Path": "/cloudfridge/Nebula-235a75b236a7c9ee/ambient_temperature"}, 
      {"Path": "/cloudfridge/Nebula-T001/cabin_temperature"}, 
      {"Path": "/cloudfridge/Nebula-235a75b236a7c9ee/evaporator_temperature"}, 
      {"Path": "/cloudfridge/Nebula-T001/evaporator_temperature"}, 
      {"Path": "/cloudfridge/Nebula-235a75b236a7c9ee/ambient_humidity"}
    ]

Data streams of a Cloudfridge device (deviceid = T-002):

`$ curl -XPOST -d 'select Path, uuid where Path like "/cloudfridge/T-002/%"' http://ard.cloudfridge.io:8079/api/query`

POST http://ard.cloudfridge.io:8079/api/query
select Path, uuid where Path like '/cloudfridge/T-002/%' 

    [
      {
        "Path": "/cloudfridge/T-002/compressor_power", 
        "uuid": "6225f168-c127-4b53-8bc1-a212b952e387"
       }, 
      {
        "Path": "/cloudfridge/T-002/evaporator_power", 
        "uuid": "325c5c66-bd23-4a5e-905f-06990922c5f4"
       }
    ]

Data values of a certain device within a time range:

POST http://ard.cloudfridge.io:8079/api/query
select data in ("5/1/2013", "6/17/2013" ) limit 10 where Path = '/cloudfridge/Nebula-235a75b236a7c9ee/cabin_temperature'

    [
      {
        "uuid": "52639e21-0302-408c-9d8e-e1f93c7992c7", 
        "Readings": [
                      [1370793022000.0, 23.6875], 
                      [1370798111000.0, 23.5], 
                      [1370798772000.0, 23.5625], 
                      [1370799491000.0, 23.625], 
                      [1370799591000.0, 23.625], 
                      [1370799852000.0, 23.6875], 
                      [1370803032000.0, 23.875], 
                      [1370804991000.0, 23.6875], 
                      [1370805693000.0, 3.5], 
                      [1370805867000.0, 5.0]
                    ]
        }
    ]



