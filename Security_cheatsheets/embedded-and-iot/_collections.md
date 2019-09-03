# Searchable FCC ID Database
- https://fccid.io/

# Command to download all FCC ID documents
```
for i in $(seq 3 4200000); do curl –referer ‘https://apps.fcc.gov/oetcf/eas/reports/ViewExhibitReport.cfm’ ‘https://apps.fcc.gov/eas/GetApplicationAttachment.html?calledFromFrame=Y&id=’$i -o $i; done
```
> https://rehmann.co/blog/command-download-fcc-id-documents/
