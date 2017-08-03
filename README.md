#Automated exports for ArchivesSpace
These scripts export updated data from ArchivesSpace and version all data in git.

##Dependencies

*   [Python 2.7 or higher](https://www.python.org/) Make sure you install the correct version. On some operating systems, this may require additional steps. It is also helpful to have [pip](https://pypi.python.org/pypi/pip) installed.
*   [lxml](http://lxml.de/)
*   [requests](http://www.python-requests.org/en/latest/)
*   [requests_toolbelt](https://github.com/sigmavirus24/requests-toolbelt)
*   [psutil](https://github.com/giampaolo/psutil)

##Getting Started

1.  Install dependencies
2.  Get a copy of the repo
3.  Create a local configuration file named `local_settings.cfg` in the same directory as the script and add variables. A sample file looks like this:

        [ArchivesSpace]
        baseURL:http://localhost:8089
        repository:2
        user:admin
        password:admin

        [EADexport]
        exportUnpublished:false
        exportDaos:true
        exportNumbered:false
        exportPdf:false

        [LastExport]
        filepath:lastExport.pickle

        [Logging]
        filename:log.txt
        format: %(asctime)s %(message)s
        datefmt: %m/%d/%Y %I:%M:%S %p
        level: WARNING

        [Destinations]
        dataDestination = /exports/data
        EADdestination = /exports/data/ead
5.  Set a cron job to run asExportIncremental.py at an interval of your choice.

The first time you run this, the script may take some time to execute, since it will attempt to export all published resource records in your ArchivesSpace repository. If you ever want to do a complete export, simply delete the Pickle file specified in `lastExportFilepath` and the `lastExport` variable will be set to zero (i.e. the epoch, which was long before ArchivesSpace was a twinkle in [anarchivist's](https://github.com/anarchivist) eye).

##Optional arguments
The script supports a few arguments, which will include or exclude specific functions.

`--update_time` updates last exported time stored in external file to current time. Useful when you want to avoid exporting everything after you re-sequence your AS instance.

##What's here

###asExportIncremental.py
Exports EAD files from published resource records updated since last export (including updates to any child components or associated agents and subjects), as well as METS records for digital object records associated with those resource records. If a resource record is unpublished, this script will remove the EAD, PDF and any associated METS records. Exported or deleted files are logged to a text file `log.txt`. (Python)

##License
This code is released under the MIT License. See `LICENSE.md` for more information. This is a stripped down version of the asExportIncremental tool from the Rockefeller Archive Center. See https://github.com/RockefellerArchiveCenter/asExportIncremental
