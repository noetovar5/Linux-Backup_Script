# Linux-Backup_Script
Linux bash backup script
Linux Automated Backup Script
A Bash-based automation that backs up directories into compressed .tar.gz files with timestamps and automatically cleans up old backups.

Features
Automatically backs up important directories
Compresses files to save space
Adds timestamps for versioning
Deletes backups older than 7 days
Simple to schedule with cron
⚙️ Usage
Edit the script and update the source directory:

SOURCE_DIR="/path/to/your/folder"
Make the script executable:

chmod +x backup_script.sh

Run the script:

./backup_script.sh

(Optional) Schedule daily backups via cron:

crontab -e 0 2 * * * /path/to/backup_script.sh

########
#!/bin/bash
# ===========================================================
#  Automated Backup Script
#  Author: Prashanth Teja
#  Description: Creates compressed backups of given directories
#               and stores them in a backup folder with timestamp.
# ===========================================================

# Variables
SOURCE_DIR="/home/$USER/Documents"
BACKUP_DIR="/home/$USER/Backups"
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_FILE="$BACKUP_DIR/backup_$TIMESTAMP.tar.gz"

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"

# Create the backup
echo "Creating backup of $SOURCE_DIR..."
tar -czf "$BACKUP_FILE" "$SOURCE_DIR" 2>/dev/null

# Verify if backup succeeded
if [ $? -eq 0 ]; then
    echo "✅ Backup successful!"
    echo "📦 Saved as: $BACKUP_FILE"
else
    echo "❌ Backup failed!"
fi

# Optional: Delete backups older than 7 days
find "$BACKUP_DIR" -type f -mtime +7 -name "*.tar.gz" -exec rm -f {} \;

echo "🧹 Old backups (older than 7 days) cleaned up."
echo "Backup process completed on $(date)"
