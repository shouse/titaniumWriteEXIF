	// Blob to binary stream
	var blobStream = Ti.Stream.createStream({ source: _data.media, mode: Ti.Stream.MODE_READ });
	var newBuffer = Ti.createBuffer({ length: _data.media.length });
	var bytes = blobStream.read(newBuffer);

	//var imageToString = _data.media.toString("binary");
	var exifRW = require('piexifjs');

	var gps = {};
	gps[exifRW.GPSIFD.GPSDateStamp] = "1999:99:99 99:99:99";

	var exifObj = {
		"GPS":gps
	};
	var exifbytes = exifRW.dump(exifObj);

	var newImageString = exifRW.insert(exifbytes, bytes);
	
	var newBlob = newImageString.toBlob();
