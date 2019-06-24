```sh
crontab -l

0 11 * * * cd /home/zhaoningbo/ops && sh ops_brain_views_v3.sh
0 6 * * * cd /home/zhaoningbo/geofence && sh shell_one_tmp.sh
0 6 * * * cd /home/zhaoningbo/geofence_penalty && sh geo_fence_penalty_exchange_i_d.sh
0 6 * * * sh /home/zhaoningbo/geo_geojson_to_text.sh
0 6 * * * sh /home/zhaoningbo/geofence/dm_geodata_dsr_by_scope.sh
0 6 * * * cd /home/zhaoningbo/ops && sh dm_ops_batch_status_change_by_day.sh
0 6 * * * cd /home/zhaoningbo/ops && sh dm_ops_healthy_to_fault_by_day.sh
0 12 * * * cd /home/zhaoningbo/ops && sh ops_brain_biketype_by_month.sh
0 6 * * * cd /home/zhaoningbo/ops && sh dm_ops_increase_fault_by_day.sh

```