var imageToString = _data.media.toString("binary");
	var exifRW = require('piexifjs');

	var gps = {};
	gps[exifRW.GPSIFD.GPSDateStamp] = "1999:99:99 99:99:99";

	var exifObj = {
		"GPS":gps
	};
	var exifbytes = exifRW.dump(exifObj);
