# DFIR Timeline cheatsheet

## Live timeline

On the computer to analyze, drop the Sleuthkit repository.

On the bin folder, launch the following command: `fls –m ‘C:/’ –f ntfs –r \\.\C: > “path_to_output_file”`

The -m option will create an output file in BODYFILE format. With this file you can then analyse it with mactime in order to generate a csv more easily readable: `mactime.pl –d –b bodyfile > timeline.csv`

## Cold timeline

Install plaso

For ubuntu: https://plaso.readthedocs.io/en/latest/sources/user/Ubuntu-Packaged-Release.html

```
sudo add-apt-repository ppa:gift/stable
sudo apt-get update
sudo apt-get install plaso-tools
```

Create the timeline: `og2timeline.py --workers 4 --vss_stores all --OUTPUT INPUT`

## Super timeline

Download: https://blogs.sans.org/computer-forensics/files/2012/01/TIMELINE_COLOR_TEMPLATE.zip

- switch to Color Timeline worksheet/tab
- Click on Cell A-1
- Select 'Data Ribbon'
- Import Data "From Text"
- Select the timeline.csv file

https://digital-forensics.sans.org/blog/2012/01/25/digital-forensic-sifting-colorized-super-timeline-template-for-log2timeline-output-files/

