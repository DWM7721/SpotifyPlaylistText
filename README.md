# SpotifyPlaylistText
Get JSON Object of song name, artist, album, date added and duration from Spotify Web Player for your playlists.

## Disclaimer
This is very rudimentary so it can only be run through Chrome Dev tools atm. (probably Firefox too but I've only tested it on Chrome)

## What is This?
A simple script that can be run in the Browser Dev Tools console to get all the song info from a Spotify playlist.
It will return a JSON Object with the Name, Artist, Album, Date Added, and Duration

## Limitations
This script is very basic and hacky. Spotify only loads a certain number of playlist entries based on the user's viewport so if the playlist is very large, the script may not get all the entries. To get around this right now, zoom out as far as possible, and/or use a vertical monitor (if possible) to maximize the loaded songs.

I already have an idea to fix this by using the playlist song number instead of a generated index for the keys.

## How To

 1. Open Dev Tools with `F11` or `Ctrl + Shift + I`
 2. Navigate to the `Console` tab, paste this code, and press enter to run
 3. Save the JSON Object by right clicking it and selecting `Copy object`
 4. Paste it somewhere to save it
 
```javascript
let results = {name:{},artist:{},album:{},dateAdded:{},duration:{}};
let songList = document.querySelectorAll('div[data-testid="playlist-tracklist"]')[0].childNodes[1].childNodes[1].childNodes;
songList.forEach((e,i) => {
	results["name"][i] = e.childNodes[0].childNodes[1].childNodes[1].childNodes[0].textContent
	let artFin = '';
	//if song is explicit, the "E" adds another child to parent
	//Explicit will be the 2nd child out of 3
	let isExplicit = e.childNodes[0].childNodes[1].childNodes[1].children.length
	if (isExplicit > 2) {
		e.childNodes[0].childNodes[1].childNodes[1].childNodes[2].childNodes.forEach((e) => artFin = artFin.concat(e.textContent));
	} else {
		e.childNodes[0].childNodes[1].childNodes[1].childNodes[1].childNodes.forEach((e) => artFin = artFin.concat(e.textContent));
	}
	results["artist"][i] = artFin;
	results["album"][i] = e.childNodes[0].childNodes[2].childNodes[0].textContent
	results["dateAdded"][i] = e.childNodes[0].childNodes[3].childNodes[0].textContent
	results["duration"][i] = e.childNodes[0].childNodes[4].childNodes[1].textContent
})
console.log(results)
```

