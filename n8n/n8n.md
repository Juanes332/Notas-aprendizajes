---
id: 20250428
---
## Installation

To install n8n we can use the docker installation:

```bash
# Create a docker volume for n8n
docker volume create n8n_data

# Download n8n image and start the container
docker run --rm --name n8n -dp 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n

# Finally go to
http://localhost:5678
```

---


## First Automation - Automatically Save Bookings from on Form submit in Airtable

**Creating an airtable form submit**

1. Add a On Form Submission Node
2. Configure the form

	![[Pasted image 20250429150322.png]]


3. Add Airtable (Create record), for this we need create an access token and added to n8n

	![[Pasted image 20250429150440.png]]

	In the schema we can drag the items that we need for the parameters in airtable

4.  Finally test it using the button test workflow

	Result:
	![[Pasted image 20250429150656.png]]




## Importing, Exporting and selling Workflows as JSON

In the navigation bar we can found a three dots, when we click on it you will find an options like Duplicate, Download, Import from URL and Import from file. 
When we want export the workflow and selling, we only need download the workflow as json and upload it in the templates section. And finally we can import the workflows with the import from file option if we have the JSON file in our pc.

## Automatically Backing up Airtable data locally

![[Pasted image 20250429155002.png]]
 
The schedule trigger only works if the switch button is in active and not in inactive.
