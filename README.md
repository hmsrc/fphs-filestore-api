# FPHS Filestore API

This repo contains scripts to perform common actions against the FPHS Filestore.

## Prerequisites

The following prerequisites must be available for these scripts to run:

- curl
- awk
- md5sum

## Uploading a directory of files to containers based on an ID

The script `upload-to-external-id-container` runs through all the files in a specified directory, using the filename on each file to look up the container to upload to. The file may be renamed with a suffix prior to uploading. The upload is then performed.

On the first script run, sub-directories will be created within the upload directory for files in specific statuses:

- failed
- in-progress
- success

For example, to upload to MRI containers on IPA Filestore, using filenames formatted like `<ipa-id>.tar.gz` to identify the IPA participant to upload to.
Each file will have the suffix "_diffusion" added prior to upload, so they appear as `<ipa-id>_diffusion.tar.gz`

```bash
# The user credentials
export upload_user_email='<api username>'
export upload_user_token='<private token>'
# The directory to upload from
upload_dir='<absolute path to directory>'

# The server to upload to
export upload_server="https://filestore.fphs.link"
# Add _diffusion to the filename before uploading
export fn_suffix='_diffusion'
# Sets the IPA app
export upload_app_type=7
# Sets up the container types to load to
export activity_log_type='activity_log__ipa_assignment_session_filestore'
export container_name='mri'
# File types to upload
export file_ext='.tar.gz'
# External ID attribute to find containers on
export external_id_attribute='ipa_id'
# Resource name for the report used to match IDs to containers
export report_name='ipa_files_api__ipa_find_container'

# Now run the script
./upload-to-external-id-container.sh ${upload_dir}
```
