# Backup-EC2-instance-self-MongoDB-data-by-schedule

It seems like there is no existing crontab for the `ubuntu` user, which might happen if you haven't set one up before. This means the cron job wasn't saved, or something went wrong during the process.

### Steps to Fix It:

1. **Re-edit the Crontab**:
   Run the `crontab -e` command again to re-open the crontab editor:
   ```bash
   crontab -e
   ```

2. **Add the Cron Job Again**:
   Add the cron job for MongoDB backup at the bottom of the file:
   ```
   0 2 * * * docker exec -t mongodb mongodump --archive=/backup/mongo_backup_$(date +\%F).gz --gzip --username admin --password ce18046 --authenticationDatabase admin && docker cp mongodb:/backup/mongo_backup_$(date +\%F).gz ~/home/backup/mongo_backup_$(date +\%F).gz
   ```

3. **Save and Exit**:
   - Press `CTRL+X` to exit the editor.
   - When prompted to save, press `Y`.
   - Press `Enter` to confirm.

4. **Verify Cron Job**:
   After saving the file, verify if the cron job was saved properly:
   ```bash
   crontab -l
   ```

It looks like the cron job has been successfully added to your crontab!

To verify everything is working as expected:

1. **Check the Cron Job Log**:
   You can check the cron job logs to see if it runs as expected. Logs for cron jobs are usually stored in `/var/log/syslog` (on Ubuntu systems). To see if the backup runs successfully, you can use this command:

   ```bash
   grep CRON /var/log/syslog
   ```

2. **Test the Cron Job Manually**:
   To ensure that the command works without waiting for the scheduled time, you can manually run the cron command in your terminal:

   ```bash
   docker exec -t mongodb mongodump --archive=/backup/mongo_backup_$(date +\%F).gz --gzip --username admin --password ce18046 --authenticationDatabase admin && docker cp mongodb:/backup/mongo_backup_$(date +\%F).gz ~/home/backup/mongo_backup_$(date +\%F).gz
   ```

   This will perform a backup immediately and save it in the `~/home/backup/` directory.

Let me know if you need further assistance!

It looks like the backup ran successfully and the file was copied to the `~/home/backup/` directory!

### What happened:
1. The `mongodump` command created a backup with the name `mongo_backup_2025-03-27.gz`.
2. The `docker cp` command successfully copied the backup from the container to the host machine.

### Next steps:
- You should now see the backup file in your `~/home/backup/` directory on the EC2 instance.
  
You can verify by running:

```bash
ls ~/home/backup/
```

If the file is there, your backup is correctly stored.

Let me know if you need further assistance or modifications!

It looks like both the `mongo_backup.gz` and `mongo_backup_2025-03-27.gz` files are present in your backup directory.

- `mongo_backup.gz` is the previous backup (if scheduled via cron).
- `mongo_backup_2025-03-27.gz` is the manual backup you just triggered.

### Next Steps:
- If you want to remove the older backup and keep only the latest one, you can use the `rm` command to delete it:

  ```bash
  rm ~/home/backup/mongo_backup.gz
  ```

- You can also verify the content of the backup to ensure it's correct. To restore the backup, you can use `mongorestore` when needed.

Let me know if you need help with that or anything else!
