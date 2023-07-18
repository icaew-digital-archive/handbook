# Converting WARC files to WACZ files

This guide explains how to convert older WARC files (including those with the .gz extension) to the newer [WACZ file format](https://specs.webrecorder.net/wacz/1.1.1/). Web Archive Collection Zipped (WACZ) allows web archives to be shared and distributed by providing a predictable way of packaging up web archive data and metadata as a ZIP file. This process also helps reduce the number of files in a web archive collection (which can run into 100s of files) into a single WACZ file.

## Conversion process

- Create a 'to-combine' folder (`mkdir 'to-combine'`) and copy the warc.gz files into it.
	

- Create a 'combined' folder (`mkdir 'combined'`). Run the following to combine the WARC files into a single WARC file prior to the conversion to WACZ: 

        cat ./to-combine/*.warc.gz > ./combined/combined.warc.gz

- Use the [py-wacz](https://github.com/webrecorder/py-wacz) Python library to then create the new WACZ file from the combined WARC file:

        wacz create ./combined/combined.warc.gz -o ./combined/combined.wacz --text --detect-pages

    Before running the previous command, create a new Python virtual environment with `python3 -m venv venv` and then activating it with `. ./venv/bin/activate` as usual. Then install the library with `pip install wacz`.

    The `--text` and `--detect-pages` allows for full-text search capability when viewed with webrecorder playback software.