# -The-Shell-Script-Bash-Fury-
#!/bin/bash

# --- REBEL CONFIG --- #
AMFI_URL="https://www.amfiindia.com/spages/NAVAll.txt"
OUTPUT_TSV="amfi_nav_data.tsv"
OUTPUT_JSON="amfi_nav_data.json"

# --- DEFY THE SYSTEM --- #
echo "Extracting data from AMFI... (This might take a sec)"  

# Download NAV data and parse Scheme Name + Asset Value
curl -s "$AMFI_URL" | awk -F ';' '{print $4 "\t" $5}' | grep -v "Scheme" > "$OUTPUT_TSV"

# --- OPTIONAL: Convert TSV to JSON (Because JSON is Superior) --- #
awk -F '\t' 'BEGIN {print "["} NR>1 {print "  {\"Scheme\": \"" $1 "\", \"NAV\": \"" $2 "\"},"} END {print "]"}' "$OUTPUT_TSV" > "$OUTPUT_JSON"

echo "🔥 Data dumped into:"
echo "- TSV: $OUTPUT_TSV"
echo "- JSON: $OUTPUT_JSON"
