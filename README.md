# dashSCTE35
## Parse DASH Events for SCTE-35.
* Xml and Binary Schemas are Supported. 
* Powered  by threefive.

# Using:
* parsing requires just three lines of code
1. `from dashscte35 import DashSCTE35`
2. `ds =DashSCTE35()`
3. `cue = ds.parse(a_dash_xml_event)`


### Parse a Dash Xml Event with Binary SCTE35 data (Base64 in xml)
```xml
exemel="""<Event presentationTime="1550252760" duration="24" id="136">
            <Signal xmlns="http://www.scte.org/schemas/35/2016">
              <Binary>/DAhAAAAAAAAAP/wEAUAAACIf+9/fgAg9YDAAAAAAABiJjIs</Binary>
            </Signal>
          </Event>"""
```
* 3 lines of code

```py3
from dashscte35 import DashSCTE35
ds =DashSCTE35()
cue = ds.parse(exemel)
```
* output
```json
{
    "Event": {
        "presentation_time": 1550252760,
        "duration": 0.000267,
        "id": 136
    },
    "Signal": {
        "xmlns": "http://www.scte.org/schemas/35/2016"
    },
    "Binary": {
        "binary": "/DAhAAAAAAAAAP/wEAUAAACIf+9/fgAg9YDAAAAAAABiJjIs"
    }
}
{
    "info_section": {
        "table_id": "0xfc",
        "section_syntax_indicator": false,
        "private": false,
        "sap_type": "0x03",
        "sap_details": "No Sap Type",
        "section_length": 33,
        "protocol_version": 0,
        "encrypted_packet": false,
        "encryption_algorithm": 0,
        "pts_adjustment": 0.0,
        "cw_index": "0x00",
        "tier": "0x0fff",
        "splice_command_length": 16,
        "splice_command_type": 5,
        "descriptor_loop_length": 0,
        "crc": "0x6226322c"
    },
    "command": {
        "command_length": 16,
        "command_type": 5,
        "name": "Splice Insert",
        "time_specified_flag": false,
        "break_auto_return": false,
        "break_duration": 24.0,
        "splice_event_id": 136,
        "splice_event_cancel_indicator": false,
        "out_of_network_indicator": true,
        "program_splice_flag": true,
        "duration_flag": true,
        "splice_immediate_flag": false,
        "event_id_compliance_flag": true,
        "unique_program_id": 49152,
        "avail_num": 0,
        "avail_expected": 0
    },
    "descriptors": []
}
```
* returns a threefive.Cue instance
  * accessSCTE-35 data via dot notation
```awk
print(cue.command.break_duration)
24.0
```
* modify data and re-encode
```awk
cue.command.break_duration=900.0
cue.encode()

'/DAhAAAAAAAAAP/wEAUAAACIf+9/fgTT9kDAAAAAAAClb4Tq'
```

### Parse a Dash XML only SCTE-35 Event.
   
    
* Time Signal with a Segmentation Descriptor and a UPID.


```xml


that_xml = """<EventStream timescale="90000" schemeIdUri="urn:scte:scte35:2013:xml">
            <Event  duration="5310000">
              <scte35:SpliceInfoSection protocolVersion="0" ptsAdjustment="183003" tier="4095">
                <scte35:TimeSignal>
                  <scte35:SpliceTime ptsTime="3442857000"/>
                </scte35:TimeSignal>
                <scte35:SegmentationDescriptor segmentationEventId="1414668" segmentationEventCancelIndicator="false" segmentationDuration="8100000">
                  <scte35:DeliveryRestrictions webDeliveryAllowedFlag="false" noRegionalBlackoutFlag="false" archiveAllowedFlag="false" deviceRestrictions="3"/>
                  <scte35:SegmentationUpid segmentationUpidType="8" segmentationUpidLength="8" segmentationTypeId="52" segmentNum="0" segmentsExpected="0">0x2df3aad7</scte35:SegmentationUpid>
                </scte35:SegmentationDescriptor>
              </scte35:SpliceInfoSection>
            </Event>
          </EventStream>"""
``` 
* Use the same 3 lines of code
```py3

from dashscte35 import DashSCTE35 #1
ds = DashSCTE35     #2
cue =ds.parse(that_xml)    #3
```
* output 
```json
{
    "EventStream": {
        "timescale": 90000,
        "scheme_id_uri": "urn:scte:scte35:2013:xml"
    },
    "Event": {
        "duration": 59.0
    },
    "SpliceInfoSection": {
        "protocol_version": 0,
        "pts_adjustment": 2.033367,
        "tier": 4095,
        "splice_command_type": 6
    },
    "TimeSignal": {},
    "SpliceTime": {
        "pts_time": 38253.966667
    },
    "SegmentationDescriptor": {
        "segmentation_event_id": 1414668,
        "segmentation_event_cancel_indicator": false,
        "segmentation_duration": 90.0
    },
    "DeliveryRestrictions": {
        "web_delivery_allowed_flag": false,
        "no_regional_blackout_flag": false,
        "archive_allowed_flag": false,
        "device_restrictions": 3
    },
    "SegmentationUpid": {
        "segmentation_upid_type": 8,
        "segmentation_upid_length": 8,
        "segmentation_type_id": 52,
        "segment_num": 0,
        "segments_expected": 0,
        "segmentation_upid": "0x2df3aad7"
    }
}
{
    "info_section": {
        "table_id": "0xfc",
        "section_syntax_indicator": false,
        "private": false,
        "sap_type": "0x03",
        "sap_details": "No Sap Type",
        "section_length": 54,
        "protocol_version": 0,
        "encrypted_packet": false,
        "encryption_algorithm": 0,
        "pts_adjustment": 2.033367,
        "cw_index": "0x0",
        "tier": "0xfff",
        "splice_command_length": 5,
        "splice_command_type": 6,
        "descriptor_loop_length": 32,
        "crc": "0x8926251d"
    },
    "command": {
        "command_length": 5,
        "command_type": 6,
        "name": "Time Signal",
        "time_specified_flag": true,
        "pts_time": 38253.966667
    },
    "descriptors": [
        {
            "tag": 2,
            "descriptor_length": 30,
            "name": "Segmentation Descriptor",
            "identifier": "CUEI",
            "segmentation_event_id": "0x15960c",
            "segmentation_event_cancel_indicator": false,
            "segmentation_event_id_compliance_indicator": true,
            "program_segmentation_flag": true,
            "segmentation_duration_flag": true,
            "delivery_not_restricted_flag": false,
            "web_delivery_allowed_flag": false,
            "no_regional_blackout_flag": false,
            "archive_allowed_flag": false,
            "device_restrictions": "No Restrictions",
            "segmentation_duration": 90.0,
            "segmentation_upid_type": 8,
            "segmentation_upid_length": 8,
            "segmentation_upid": "0x2df3aad7",
            "segmentation_type_id": 52,
            "segment_num": 0,
            "segments_expected": 0,
            "sub_segment_num": 0,
            "sub_segments_expected": 0
        }
    ]
}
```
