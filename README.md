# dashSCTE-35
Parse DASH Events for SCTE-35 data and decode it . Xml and Binary Schemas are Supported. Powered  by threefive.

# Using:
* Binary (Base64 in xml)
```py3
from dashscte35 import DashSCTE35

exemel="""<Event presentationTime="1550252760" duration="24" id="136">
            <Signal xmlns="http://www.scte.org/schemas/35/2016">
              <Binary>/DAhAAAAAAAAAP/wEAUAAACIf+9/fgAg9YDAAAAAAABiJjIs</Binary>
            </Signal>
          </Event>"""

ds =DashSCTE35()
cue = ds.parse(exemel)
```
* output
```js
 cue = ds.parse(exemel)
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


