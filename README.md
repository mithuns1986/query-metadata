# query-metadata
Code to query metadata of an instance within AWS and provides JSON formatted output. The script uses the `curl` command to retrieve the metadata and `jq` tool to format the output as JSON.

```bash
#!/bin/bash

# Retrieve the metadata
metadata=$(curl -s http://169.254.169.254/latest/dynamic/instance-identity/document)

# Check if a specific data key is provided as an argument
if [ $# -eq 1 ]; then
  # Retrieve the value of the specified key
  value=$(echo "$metadata" | jq -r ".$1")
  
  # Output the value as JSON
  echo "{\"$1\": \"$value\"}"
else
  # Output the entire metadata as JSON
  echo "$metadata" | jq .
fi
```

To use this script, save it to a file (e.g., `metadata.sh`), make it executable (`chmod +x metadata.sh`), and run it. Optionally, you can provide a specific data key as an argument to retrieve its value individually.

Example usage:

```bash
# Retrieve and output the entire metadata
./metadata.sh

# Retrieve and output a specific data key individually
./metadata.sh instanceType
```

Explanation:

1. The script starts by using the `curl` command to retrieve the metadata from the instance metadata service. The metadata endpoint `http://169.254.169.254/latest/dynamic/instance-identity/document` provides information about the instance, such as instance ID, instance type, region, etc.

2. If an argument is provided when running the script (`$# -eq 1`), it assumes that a specific data key is provided, and it retrieves the value of that key from the metadata using `jq` tool. The `-r` flag is used with `jq` to output the value without quotes.

3. If no argument is provided, the entire metadata is outputted as JSON using `jq .`. The `.` represents the entire JSON document.

The script allows for easy retrieval of the metadata of an AWS instance in a JSON format. It can be extended to handle additional functionality as per specific requirements. Remember to run this script on an EC2 instance within AWS to access the instance metadata.
