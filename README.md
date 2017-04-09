# Dell Bios Installer

This little script will download and install the most current BIOS version from the [Dell download server](http://downloads.dell.com/published/Pages/index.html)  
  
This is accomplished by scraping the site for the name of the computer system and following the link. There the script finds the table with the BIOS downloads and receives the top result.    
The last step is executed in Powershell; via sendkeys the BIOS-Update is started and executed.  
  
The Python 3.4 script uses [robobrowser](https://github.com/jmcarp/robobrowser) for the scraping, the exe was compiled by [py2exe](https://pypi.python.org/pypi/py2exe/) and is secure
[check here](https://www.virustotal.com/de/file/cdb8c4abf2f710d25821449ad02876b566b37a04b77e8fa86263f3e16b0b7a5c/analysis/1491768604/ "Virustotal")