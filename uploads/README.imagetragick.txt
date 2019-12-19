for filename in `find . -type f | grep imagetragick`; do sed 's/PAYLOAD/<burp_collaborator_payload_id>/g' $filename > "new_$(echo $filename | rev | cut -d / -f 1 | rev)";done
