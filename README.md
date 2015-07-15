	/**
	 * @method updateExifData
	 * Update EXIF Data
	 */
	function updateExifData(_image) {
	
		var blobToBinaryString = function(_blob) {
			// Blob to binary stream
			var blobStream = Ti.Stream.createStream({ source: _blob, mode: Ti.Stream.MODE_READ });
			var newBuffer = Ti.createBuffer({ length: _blob.length });
			var bytes = blobStream.read(newBuffer);
	
			var imageToString = _blob.toString(2);
	
			console.log('[BLOB TO BINARY STRING? ]' + imageToString);
			console.log('[BLOB TO BYTES? ]' + bytes);
	
			return imageToString;
			//return bytes;
		};
	
		var blobToJpgString = function(_blob) {
			var filename = Titanium.Filesystem.applicationDataDirectory + "/image.jpg";
			f = Titanium.Filesystem.getFile(filename);
			f.write(_blob);
	
			var jpgString = f.read().toString(2);
	
			console.log('[JPG BINARY STRING? ]' + jpgString);
	
			return jpgString;
		};
	
	
		var imageString = blobToBinaryString(_image);
		var imageString2 = blobToJpgString(_image);
	
		var exifRW = require('piexifjs');
	
		var gps = {};
		gps[exifRW.GPSIFD.GPSDateStamp] = "1999:99:99 99:99:99";
	
		var exifObj = {
			"GPS":gps
		};
		var exifbytes = exifRW.dump(exifObj);
	
		//var newImageString = exifRW.insert(exifbytes, bytes);
		//var newImageString = exifRW.insert(exifbytes, _image);
		var newImageString = exifRW.insert(exifbytes, imageString2);
		var newImageString = exifRW.insert(exifbytes, imageString);
	
		var newBlob = newImageString.toBlob();
	
		return updatedImage;
	};
