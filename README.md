	
	/**
	 * @method getExifData
	 * Get EXIF Data from image
	 */
	function getExifData(_pathOrBlob, _imageOrPath) {
		var Exif = require('modules/exif');
		var data;
	
		if (_pathOrBlob == 'blob') {
			data = Exif.fromBlob(_imageOrPath);
			console.warn('[EXIF DATA FROM BLOB]' + Exif.pretty(data));
		} else if (_pathOrBlob == 'path') {
			data = Exif.fromPath(_imageOrPath);
			console.warn('[EXIF DATA FROM PATH]' + Exif.pretty(data));
		}
	
		return data;
	};
	
	
	getExifData('blob', _data.media);

	var filename = Titanium.Filesystem.applicationDataDirectory + "/image.jpg";
	f = Titanium.Filesystem.getFile(filename);
	f.write( _data.media );

	var jpgString = f.read().toString();

	// Blob to binary stream
	var blobStream = Ti.Stream.createStream({ source: _data.media, mode: Ti.Stream.MODE_READ });
	var newBuffer = Ti.createBuffer({ length: _data.media.length });
	var bytes = blobStream.read(newBuffer);

	var bytes = _data.media.toString("binary");

	//var imageToString = _data.media.toString("binary");
	var exifRW = require('piexifjs');

	var gps = {};
	gps[exifRW.GPSIFD.GPSDateStamp] = "1999:99:99 99:99:99";

	var exifObj = {
		"GPS":gps
	};
	var exifbytes = exifRW.dump(exifObj);

	//var newImageString = exifRW.insert(exifbytes, bytes);
	var newImageString = exifRW.insert(exifbytes, jpgString);

	var newBlob = newImageString.toBlob();

	console.error('[New image coming: ]');
	getExifData('blob', newBlob);
