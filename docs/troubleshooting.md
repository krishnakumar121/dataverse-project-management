# Troubleshooting Guide

## Common Issues

### Issue: Tables Not Appearing in Power Apps

**Symptoms:**
- Tables don't show up in app designer
- Model app won't load data
- "Table not found" error

**Solutions:**
1. Refresh Power Apps (F5)
2. Ensure tables are published in Dataverse
3. Check table permissions/security
4. Clear browser cache
5. Try different browser
6. Wait 5-10 minutes for sync

---

### Issue: Relationships Not Working

**Symptoms:**
- Lookup fields empty
- Related data not showing
- "Relationship not found" error

**Solutions:**
1. Verify relationship exists in Dataverse
2. Check relationship name
3. Ensure cardinality is correct (1:N)
4. Republish tables
5. Refresh app
6. Check data exists in parent table

---

### Issue: Power Automate Workflows Not Triggering

**Symptoms:**
- Emails not sending
- Tasks not creating
- Workflow runs show failed

**Solutions:**
1. Check flow is turned "On"
2. Verify trigger conditions
3. Review flow run history for errors
4. Check connections are authorized
5. Ensure user has permissions
6. Test with sample data
7. Add error handling

---

### Issue: Performance Degradation

**Symptoms:**
- App loading slowly
- Grid views take long
- Searches lag
- Mobile app sluggish

**Solutions:**
1. Archive old projects
2. Clear browser cache
3. Use filters before loading
4. Reduce columns in view
5. Pagination on large lists
6. Check internet speed
7. Close unused apps
8. Contact admin if continues

---

### Issue: Security/Permission Errors

**Symptoms:**
- "Access Denied" message
- Can't edit records
- Can't create new items
- "Insufficient permissions" error

**Solutions:**
1. Check security role assigned
2. Verify row-level security
3. Check field-level permissions
4. Ask manager/admin for access
5. Ensure user is added to team
6. Check org unit assignments

---

### Issue: Data Not Syncing

**Symptoms:**
- Changes not appearing
- Old data showing
- Offline sync issues

**Solutions:**
1. Refresh page (Ctrl+F5)
2. Ensure online connection
3. Sync mobile app
4. Check last modified date
5. Verify user has read access
6. Check if record is locked

---

### Issue: Forms Not Saving

**Symptoms:**
- Form won't save
- "Validation error" message
- Required fields flagged

**Solutions:**
1. Check required fields filled
2. Verify date formats (MM/DD/YYYY)
3. Check number formats
4. Ensure lookup fields populated
5. Check field length limits
6. Retry save
7. Clear form cache

---

## Diagnostic Steps

### Step 1: Check Connectivity

```
1. Test internet connection
2. Verify sign-in status
3. Check Power Apps status page
4. Try different network (wifi/cellular)
5. Check firewall/proxy settings
```

### Step 2: Review Logs

```
1. Check Power Automate run history
2. Review Dataverse audit logs
3. Check browser console errors (F12)
4. Look at Power Apps analytics
5. Check system jobs
```

### Step 3: Test Functionality

```
1. Create test record
2. Edit test record
3. Delete test record
4. Test lookup fields
5. Test views and filters
6. Test workflows
```

---

## Advanced Troubleshooting

### Check Dataverse Health

1. Go to **Power Platform Admin Center**
2. Select environment
3. Check **Health**
4. Review system jobs
5. Monitor capacity

### Review Security

1. Check user's security role
2. Verify field permissions
3. Review sharing settings
4. Check team membership
5. Audit access logs

### Monitor Performance

1. Use Developer Tools (F12)
2. Check network requests
3. Monitor API response times
4. Analyze query performance
5. Profile Power Automate flows

---

## Contact Support

### Before Contacting

Have ready:
- Error message (exact text)
- Steps to reproduce
- Screenshots
- Environment details
- User information
- Recent changes

### Where to Get Help

- **Internal:** Contact your administrator
- **Microsoft:** Support ticket in Admin Center
- **Community:** Power Apps Community Forum
- **Documentation:** Check docs folder

---

**Version:** 1.0  
**Last Updated:** June 10, 2026
