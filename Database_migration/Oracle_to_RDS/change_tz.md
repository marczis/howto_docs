# Change timezone on Oracle RDS

```
exec rdsadmin.rdsadmin_util.alter_db_time_zone(p_new_tz=> '+1:00');
```

From UTC.