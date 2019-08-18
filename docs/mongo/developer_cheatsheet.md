This is a cheat sheet for developers to keep mongo DB scripts / commands handy.

## Basic Commands

### Export a collection from database
    mongoexport --db pfwTrackingReportingData -c parcelData --out /home/bipnayak/parcelData.json

### Import a collection to database
    mongoimport --db pfwTrackingReportingData --collection parcelData --file parcelData.json