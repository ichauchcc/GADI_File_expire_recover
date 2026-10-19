# GADI_File_expire_recover

If system shows there are files on Gadi are about to expire soon or has already expired.  
__"You have ** files about to expire under project ****"__  
Here are some list of command useful:  
```{sh}
nci-file-expiry list-warnings --help
nci-file-expiry list-quarantined --help
nci-file-expiry recover --help
nci-file-expiry status --help
```

If the system has shown this  
__"You have ** files in quarantine under project ****"__  
Follow this process:  
```
mkdir -p /scratch/ut47/file_recovery
nci-file-expiry list-quarantined > /scratch/ut47/file_recovery/list.quarantin

# make sure it contians file names
ls -lh /scratch/ut47/file_recovery/list.quarantin
```

Then generate the recovery commands:  
```
awk '
NR==1 {next}                  # skip header
NF==0 {next}                  # skip blank lines
{
    id=$1
    path=substr($0, index($0, "/"))
    print "nci-file-expiry recover " id " \"" path "\""
}
' /scratch/ut47/file_recovery/list.quarantin > /scratch/ut47/file_recovery/recover_quarantined.sh
# Make it executable
cd /scratch/ut47/file_recovery
chmod +x recover_quarantined.sh
# Then run
bash recover_quarantined.sh
```
Then, check for the file status  
```
nci-file-expiry status --help
```
