#!/bin/bash
dateToday=`date +"%Y-%m-%d"`
printf "* Starting Site Backup...\n"

#Read in Site Name from user and loop if no name is entered.
read -p "* Enter Site Name exactly as it appears in MPCP: " name

while [[ $name == '' ]]
do
    printf "\n* Site Name cannot be empty.\n"
    read -p "* Enter Site Name exactly as it appears in MPCP: " name
done 

#Create and move to backup directory us
backupDir="${name}-backup-${dateToday}"
mkdir -p "${backupDir}"
if [ $? -eq 0 ]; then
   printf "\n* Backup directory created.\n\n"
else
   printf "\n* Error creating directory. Exiting."
   exit
fi
cd "${backupDir}"

#Export Database.
wp-cli --skip-plugins --skip-themes db export --path=/htdocs/__wp__
if [ $? -eq 0 ]; then
    printf "\n* Database exported.\n\n"
else
   printf "\n* Error exporting database. Exiting."
   exit
fi

#Compress wp-content folder.
tar -czvf "${name}-files-${dateToday}.tar.gz" ../../htdocs/wp-content
printf "\n* wp-content directory compressed.\n\n"

#Compress backup directory.
cd ../
tar -czvf "${backupDir}.tar.gz" "${backupDir}"
printf "\n* Backup directory compressed.\n"

#Move backup archive to htdocs and provide download link.
rm -rf "${backupDir}"
printf "\n* Files cleaned up.\n"
mv "${backupDir}.tar.gz" ../htdocs
if [ $? -eq 0 ]; then
   printf "\n*** Backup available at: ${name}/${backupDir}.tar.gz ***\n"
else
   printf "\n* Error moving backup to ../htdocs. Exiting."
   exit
fi

#Remove this script.
rm site-backup.sh
