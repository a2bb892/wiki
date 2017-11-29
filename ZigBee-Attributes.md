These are the ZigBee clusters and attributes reported from some of the devices I've been using.

## GE Dimmer Lamp Module ZB3101
### Input Clusters for endpoint 1 (ProfileID 0104, DeviceId 0101)
```
0000 - genBasic
  AttrId: zclVersion (0) dataType: uint8 (32)
  AttrId: appVersion (1) dataType: uint8 (32)
  AttrId: stackVersion (2) dataType: uint8 (32)
  AttrId: hwVersion (3) dataType: uint8 (32)
  AttrId: manufacturerName (4) dataType: charStr (66)
  AttrId: modelId (5) dataType: charStr (66)
  AttrId: powerSource (7) dataType: enum8 (48)
0003 - genIdentify
  AttrId: identifyTime (0) dataType: uint16 (33)
0004 - genGroups
  AttrId: nameSupport (0) dataType: bitmap8 (24)
0005 - genScenes
  AttrId: count (0) dataType: uint8 (32)
  AttrId: currentScene (1) dataType: uint8 (32)
  AttrId: currentGroup (2) dataType: uint16 (33)
  AttrId: sceneValid (3) dataType: boolean (16)
  AttrId: nameSupport (4) dataType: bitmap8 (24)
0006 - genOnOff
  AttrId: onOff (0) dataType: boolean (16)
0008 - genLevelCtrl
  AttrId: currentLevel (0) dataType: uint8 (32)
  AttrId: onLevel (17) dataType: uint8 (32)
0b05 - haDiagnostic
  AttrId: averageMacRetryPerApsMessageSent (283) dataType: uint16 (33)
  AttrId: lastMessageLqi (284) dataType: uint8 (32)
  AttrId: lastMessageRssi (285) dataType: int8 (40)
0702 - seMetering
  AttrId: currentSummDelivered (0) dataType: uint48 (37)
  AttrId: status (512) dataType: bitmap8 (24)
  AttrId: unitOfMeasure (768) dataType: enum8 (48)
  AttrId: multiplier (769) dataType: uint24 (34)
  AttrId: divisor (770) dataType: uint24 (34)
  AttrId: summaFormatting (771) dataType: bitmap8 (24)
  AttrId: demandFormatting (772) dataType: bitmap8 (24)
  AttrId: historicalConsumpFormatting (773) dataType: bitmap8 (24)
  AttrId: meteringDeviceType (774) dataType: bitmap8 (24)
  AttrId: instantaneousDemand (1024) dataType: int24 (42)
```
### Output Clusters for endpoint 1 (ProfileID 0104, DeviceId 0101)
```
000a - genTime
0019 - genOta
```
### Input Clusters for endpoint 2 (ProfileID 0104, DeviceId 0104)
```
0000 - genBasic
  AttrId: zclVersion (0) dataType: uint8 (32)
  AttrId: appVersion (1) dataType: uint8 (32)
  AttrId: stackVersion (2) dataType: uint8 (32)
  AttrId: hwVersion (3) dataType: uint8 (32)
  AttrId: manufacturerName (4) dataType: charStr (66)
  AttrId: modelId (5) dataType: charStr (66)
  AttrId: powerSource (7) dataType: enum8 (48)
0003 - genIdentify
  AttrId: identifyTime (0) dataType: uint16 (33)
0b05 - haDiagnostic
  AttrId: averageMacRetryPerApsMessageSent (283) dataType: uint16 (33)
  AttrId: lastMessageLqi (284) dataType: uint8 (32)
  AttrId: lastMessageRssi (285) dataType: int8 (40)
```
### Output Clusters for endpoint 2 (ProfileID 0104, DeviceId 0104)
```
0003 - genIdentify
  AttrId: identifyTime (0) dataType: uint16 (33)
0006 - genOnOff
0008 - genLevelCtrl
```
## GE On/Off Light and Small Appliance Module ZB4101
### Input Clusters for endpoint 1 (ProfileID 0104, DeviceId 0100)
```
0000 - genBasic
  AttrId: zclVersion (0) dataType: uint8 (32)
  AttrId: appVersion (1) dataType: uint8 (32)
  AttrId: stackVersion (2) dataType: uint8 (32)
  AttrId: hwVersion (3) dataType: uint8 (32)
  AttrId: manufacturerName (4) dataType: charStr (66)
  AttrId: modelId (5) dataType: charStr (66)
  AttrId: powerSource (7) dataType: enum8 (48)
0003 - genIdentify
0004 - genGroups
  AttrId: identifyTime (0) dataType: uint16 (33)
  AttrId: nameSupport (0) dataType: bitmap8 (24)
0005 - genScenes
  AttrId: count (0) dataType: uint8 (32)
  AttrId: currentScene (1) dataType: uint8 (32)
  AttrId: currentGroup (2) dataType: uint16 (33)
  AttrId: sceneValid (3) dataType: boolean (16)
  AttrId: nameSupport (4) dataType: bitmap8 (24)
0006 - genOnOff
  AttrId: onOff (0) dataType: boolean (16)
0b05 - haDiagnostic
  AttrId: averageMacRetryPerApsMessageSent (283) dataType: uint16 (33)
  AttrId: lastMessageLqi (284) dataType: uint8 (32)
  AttrId: lastMessageRssi (285) dataType: int8 (40)
0702 - seMetering
  AttrId: currentSummDelivered (0) dataType: uint48 (37)
  AttrId: status (512) dataType: bitmap8 (24)
  AttrId: unitOfMeasure (768) dataType: enum8 (48)
  AttrId: multiplier (769) dataType: uint24 (34)
  AttrId: divisor (770) dataType: uint24 (34)
  AttrId: summaFormatting (771) dataType: bitmap8 (24)
  AttrId: demandFormatting (772) dataType: bitmap8 (24)
  AttrId: historicalConsumpFormatting (773) dataType: bitmap8 (24)
  AttrId: meteringDeviceType (774) dataType: bitmap8 (24)
  AttrId: instantaneousDemand (1024) dataType: int24 (42)
```
### Output Clusters for endpoint 1 (ProfileID 0104, DeviceId 0100)
```
0003 - genIdentify
  AttrId: identifyTime (0) dataType: uint16 (33)
000a - genTime
0019 - genOta
```
### Input Clusters for endpoint 2 (ProfileID 0104, DeviceId 0103)
```
0000 - genBasic
  AttrId: zclVersion (0) dataType: uint8 (32)
  AttrId: appVersion (1) dataType: uint8 (32)
  AttrId: stackVersion (2) dataType: uint8 (32)
  AttrId: hwVersion (3) dataType: uint8 (32)
  AttrId: manufacturerName (4) dataType: charStr (66)
  AttrId: modelId (5) dataType: charStr (66)
  AttrId: powerSource (7) dataType: enum8 (48)
0003 - genIdentify
  AttrId: identifyTime (0) dataType: uint16 (33)
0b05 - haDiagnostic
  AttrId: averageMacRetryPerApsMessageSent (283) dataType: uint16 (33)
  AttrId: lastMessageLqi (284) dataType: uint8 (32)
  AttrId: lastMessageRssi (285) dataType: int8 (40)
```
### Output Clusters for endpoint 2 (ProfileID 0104, DeviceId 0103)
```
0003 - genIdentify
  AttrId: identifyTime (0) dataType: uint16 (33)
0006 - genOnOff
```

## CentralLite 4256050-RZHAC
### Input Clusters for endpoint 1 (ProfileID 0104, DeviceId 0100)
```
0000 - genBasic
  AttrId: zclVersion (0) dataType: uint8 (32)
  AttrId: appVersion (1) dataType: uint8 (32)
  AttrId: hwVersion (3) dataType: uint8 (32)
  AttrId: manufacturerName (4) dataType: charStr (66)
  AttrId: modelId (5) dataType: charStr (66)
  AttrId: dateCode (6) dataType: charStr (66)
  AttrId: powerSource (7) dataType: enum8 (48)
0003 - genIdentify
  AttrId: identifyTime (0) dataType: uint16 (33)
0004 - genGroups
  AttrId: nameSupport (0) dataType: bitmap8 (24)
0005 - genScenes
  AttrId: count (0) dataType: uint8 (32)
  AttrId: currentScene (1) dataType: uint8 (32)
  AttrId: currentGroup (2) dataType: uint16 (33)
  AttrId: sceneValid (3) dataType: boolean (16)
  AttrId: nameSupport (4) dataType: bitmap8 (24)
0006 - genOnOff
  AttrId: onOff (0) dataType: boolean (16)
0b04 - haElectricalMeasurement
  AttrId: measurementType (0) dataType: bitmap32 (27)
  AttrId: acFrequency (768) dataType: uint16 (33)
  AttrId: rmsVoltage (1285) dataType: uint16 (33)
  AttrId: rmsCurrent (1288) dataType: uint16 (33)
  AttrId: activePower (1291) dataType: int16 (41)
  AttrId: acCurrentMultiplier (1538) dataType: uint16 (33)
  AttrId: acCurrentDivisor (1539) dataType: uint16 (33)
  AttrId: acPowerMultiplier (1540) dataType: uint16 (33)
  AttrId: acPowerDivisor (1541) dataType: uint16 (33)
0b05 - haDiagnostic
  AttrId: numberOfResets (0) dataType: uint16 (33)
  AttrId: macRxBcast (256) dataType: uint16 (33)
  AttrId: macTxBcast (257) dataType: uint16 (33)
  AttrId: macRxUcast (258) dataType: uint16 (33)
  AttrId: macTxUcast (259) dataType: uint16 (33)
  AttrId: macTxUcastRetry (260) dataType: uint16 (33)
  AttrId: macTxUcastFail (261) dataType: uint16 (33)
  AttrId: aPSRxBcast (262) dataType: uint16 (33)
  AttrId: aPSTxBcast (263) dataType: uint16 (33)
  AttrId: aPSRxUcast (264) dataType: uint16 (33)
  AttrId: aPSTxUcastSuccess (265) dataType: uint16 (33)
  AttrId: aPSTxUcastRetry (266) dataType: uint16 (33)
  AttrId: aPSTxUcastFail (267) dataType: uint16 (33)
  AttrId: routeDiscInitiated (268) dataType: uint16 (33)
  AttrId: neighborAdded (269) dataType: uint16 (33)
  AttrId: neighborRemoved (270) dataType: uint16 (33)
  AttrId: neighborStale (271) dataType: uint16 (33)
  AttrId: joinIndication (272) dataType: uint16 (33)
  AttrId: childMoved (273) dataType: uint16 (33)
  AttrId: nwkFcFailure (274) dataType: uint16 (33)
  AttrId: apsFcFailure (275) dataType: uint16 (33)
  AttrId: apsUnauthorizedKey (276) dataType: uint16 (33)
  AttrId: nwkDecryptFailures (277) dataType: uint16 (33)
  AttrId: apsDecryptFailures (278) dataType: uint16 (33)
  AttrId: packetBufferAllocateFailures (279) dataType: uint16 (33)
  AttrId: relayedUcast (280) dataType: uint16 (33)
```
### Output Clusters for endpoint 1 (ProfileID 0104, DeviceId 0100)
```
0019 - genOta
```
