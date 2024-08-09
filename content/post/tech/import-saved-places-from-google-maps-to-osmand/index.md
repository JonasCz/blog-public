---
title: "How to export saved places from Google Maps as GPX for OsmAnd"
date: 2024-08-08T12:59:10+01:00
draft: false
image: screenshot.jpg
categories:
- Apps & software
tags:
- OsmAnd
---
For my next big bike trip, I've been collecting interesting places that I might want to visit, using Google Maps. Now, I'd like to import them into OsmAnd, my offline navigation app of choice. OsmAnd can import GPX files, however Google maps won't let you export your saved places in a usable format such as GPX, which I'm guessing is because they'd rather you not leave Google Maps and take your data with you.

There is however Google Takout, which does offer a CSV file with links to Google Maps and not much else. This needs to be converted to a GPX file with coordinates to be able to use it with another app such as OsmAnd.

**1.** Go to [Google Takeout](https://takeout.google.com/), then choose "deselect all" at the top of the list (to ensure you're only getting your Maps data), then select "Saved" (Not any of the other "Maps" related items). Proceed with the Google Takeout export, at then end you'll be emailed a zip file containing a "Saved" folder, with a csv file inside for each of your place lists.

**2.** Upload this csv file to [https://exportgooglemaps.com/](https://exportgooglemaps.com/). Disclaimer: This isn't my service, and it doesn't have a privacy policy that I can see, so it's up to you weather you trust it with your places.

Pick "JSON" when you download after it finishes.

**3.** Upload the resulting JSON file below, to convert it to a GPX file. This one works locally in your browser and doesn't send your data anywhere. You may include a "type" field, this will group the places together as one folder in OsmAnd, otherwise they will be in separate folders based on how Google Maps has categorized them.

<div>
    <h1>JSON to GPX Converter</h1>
    <input type="file" id="fileInput" accept=".json">
	<!-- Add this new input field for type -->
	<br>
<label for="typeInput">Type (optional):</label>
<input type="text" id="typeInput" placeholder="Enter type for all items">
<br>
 <button onclick="convertToJson()">Convert to GPX</button>
 <br>
    <a id="downloadLink" style="display:none;">Download GPX</a>

    <script>
        function convertToJson() {
    const fileInput = document.getElementById('fileInput');
    const customType = document.getElementById('typeInput').value || "Unknown";
    const file = fileInput.files[0];

    if (!file) {
        alert("Please select a file first!");
        return;
    }

    const reader = new FileReader();
    reader.onload = function(event) {
        const jsonData = JSON.parse(event.target.result);
        const skippedItems = [];
        const gpxData = jsonToGpx(jsonData, skippedItems, customType);
        
        if (skippedItems.length > 0) {
            alert("Skipped the following items due to missing GPS data:\n" + skippedItems.join('\n'));
        }

        const blob = new Blob([gpxData], { type: 'application/gpx+xml' });
        const url = URL.createObjectURL(blob);

        const downloadLink = document.getElementById('downloadLink');
        downloadLink.href = url;
        downloadLink.download = 'converted.gpx';
        downloadLink.style.display = 'block';
        downloadLink.textContent = 'Download GPX File';
    };
    reader.readAsText(file);
}

function jsonToGpx(jsonData, skippedItems, customType) {
    let gpxContent = `<?xml version="1.0" encoding="UTF-8"?>
<gpx version="1.1" creator="JSON to GPX Converter">
`;

    jsonData.forEach(entry => {
        if (!entry.gps || !entry.gps.lat || !entry.gps.lng) {
            skippedItems.push(entry.name || "Unnamed item");
            return;
        }

        const name = encodeXml(entry.name || "Unnamed");
        const lat = entry.gps.lat;
        const lon = entry.gps.lng;
        const description = encodeXml(entry.description || "No description available.");
        const type = encodeXml(customType);

        gpxContent += `
<wpt lat="${lat}" lon="${lon}">
    <name>${name}</name>
    <desc>${description}</desc>
    <type>${type}</type>
</wpt>`;
    });

    gpxContent += `
</gpx>`;
    return gpxContent;
}

function encodeXml(text) {
    return text.replace(/&/g, "&amp;")
               .replace(/</g, "&lt;")
               .replace(/>/g, "&gt;")
               .replace(/"/g, "&quot;")
               .replace(/'/g, "&apos;");
}

    </script>
</div>

Once you have your GPX file, go to "My Places" in OsmAnd and choose + (add), and pick the GPX file you downloaded above.