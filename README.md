# How to solve the error message Unsupported charset or collation for tables after a zabbix upgrade
Solve the zabbix database error message "Unsupported charset or collation for tables"

After my upgrade i got the message on my dashboard
```
Unsupported charset or collation for tables: host_discovery, history_str, media_type_param, graphs_items, media, httpstep_field, application_prototype, hostmacro, history_log, proxy_history, tag_filter, expressions, httpstep, interface, problem, globalmacro, sysmap_shape, acknowledges, group_prototype, icon_map, item_preproc, sysmap_url, hosts, httptest, profiles, history_text, services, maintenance_tag, ids, sessions, httptest_field, opconditions, proxy_dhistory, functions, sysmaps_link_triggers, users, graphs, auditlog_details, config_autoreg_tls, scripts, config, application_discovery, alerts, triggers, sysmap_element_url, sysmaps, dchecks, images, media_type, graph_theme, sysmaps_links, screens_items, corr_condition_tag, corr_condition_tagpair, task_remote_command_result, usrgrp, housekeeper, item_condition, slideshows, drules, items, item_rtdata, problem_tag, correlation, proxy_autoreg_host, trigger_tag, icon_mapping, actions, maintenances, host_inventory, services_times, screens, item_discovery, event_tag, corr_condition_tagvalue, operations, lld_macro_path, conditions, events, regexps, group_discovery, sysmaps_elements, mappings, widget_field, task_remote_command, opcommand, opmessage, slides, autoreg_host, host_tag, dashboard, valuemaps, dservices, applications, auditlog, hstgrp, widget.
```

The easiest way of solving this is by making a mysql-dump and search and replace the wrong collations

## Step 1 - stop zabbix server
```
systemctrl stop zabbix-server
```

## Step 2 - make a sqldump
```
mysqldump -uroot -p zabbix > zabbix_dump.sql
```

## step 3 - search and replace from utf8mb4_general_ci to utf8mb4_bin
```
sed 's/utf8mb4_general_ci/utf8mb4_bin/g' zabbix_dump.sql  > new_zabbix_dump.sql
```

## Step 4 - import new made database
```
mysql -uroot -p zabbix -f < new_zabbix_dump.sql
```

## Step 5 - Edit file /etc/zabbix/web/zabbix.conf.php and set $DB['DOUBLE_IEEE754'] in true
```
$DB['DOUBLE_IEEE754']           = true;
```


##
