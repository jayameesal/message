# message

#!/bin/bash

set -x

 . /usr/share/3pl-crons/db/db_config.sh

 CURR_TIME=$(date +"%Y-%m-%d_%H-%M-%S")
 DATE=`date +"%F" `

 shipping_lite_1_ws_user=shipping_lite_1_ws_ro
 shipping_lite_1_ws_pwd=nMiTqb175Q
 shipping_lite_1_ws_host=10.24.32.22
 shipping_lite_1_ws_db=shipping_lite_1
 shipping_lite_1_ws_port=4000

 shipping_lite_2_ws_user=shipping_lite_2_ws_ro
 shipping_lite_2_ws_pwd=rsr4p0pQee
 shipping_lite_2_ws_host=10.24.32.22
 shipping_lite_2_ws_db=shipping_lite_2
 shipping_lite_1_ws_port=4000

 shipping_lite_3_ws_user=shipping_lite_3_ws_ro
 shipping_lite_3_ws_pwd=hINYqZ9FtV
 shipping_lite_3_ws_host=10.24.32.22
 shipping_lite_3_ws_db=shipping_lite_3
 shipping_lite_1_ws_port=4000

 shipping_lite_4_ws_user=shipping_lite_4_ws_ro
 shipping_lite_4_ws_pwd=VbOz5Ake2B
 shipping_lite_4_ws_host=10.24.32.22
 shipping_lite_4_ws_db=shipping_lite_4
 shipping_lite_1_ws_port=4000

 shipping_lite_5_ws_user=shipping_lite_5_ws_ro
 shipping_lite_5_ws_pwd=ApE5hTRtce
 shipping_lite_5_ws_host=10.24.32.22
 shipping_lite_5_ws_db=shipping_lite_5
 shipping_lite_1_ws_port=4000

 shipping_lite_6_ws_user=shipping_lite_6_ws_ro
 shipping_lite_6_ws_pwd=CXl4Fhs4b7
 shipping_lite_6_ws_host=10.24.32.22
 shipping_lite_6_ws_db=shipping_lite_6
 shipping_lite_1_ws_port=4000


 export MAILFROM="Shipping-Dev  <bikrant.sahoo@flipkart.com>"
 export MAILTO="jaya.meesal.vc@flipkart.com"
 export SUBJECT="lite_message_id"


 CWD="/home/jaya.meesal.vc/"
 input_file_name=$CWD/s
 comma_separated_file_input="commaseparated_data_$DATE"

 awk -F "<>" '{print $1}' $input_file_name |awk '{printf("'"'"'%s'"'"',", $0)} END{print ""}' |sed -e 's/^/'\('/g' |sed -e 's/$/'\)'/g' |sed 's/\,)/\)/' > $comma_separated_file_input

 echo "$comma_separated_file_input"

 Query="SELECT s.vendorTrackingId, sh.status
 FROM Shipments s
 JOIN ShipmentStatusHistories sh ON s.Id = sh.shipmentId
 WHERE s.vendorTrackingId IN $(cat $comma_separated_file_input) and sh.status in ('rvp_cancelled','RTO_Completed','rvp_handover_completed');"

 echo "$Query" | mysql -N -u$decoded_shippinglite1_user -p$decoded_shipping_lite_1_pass -h$shippinglite1_ro_ip shipping_lite_${shard} >>$CWD/output1.csv
 echo "$Query" | mysql -N -u$decoded_shippinglite2_user -p$decoded_shipping_lite_2_pass -h$shippinglite2_ro_ip shipping_lite_${shard} >>$CWD/output2.csv
 echo "$Query" | mysql -N -u$decoded_shippinglite3_user -p$decoded_shipping_lite_3_pass -h$shippinglite3_ro_ip shipping_lite_${shard} >>$CWD/output3.csv
 echo "$Query" | mysql -N -u$decoded_shippinglite4_user -p$decoded_shipping_lite_4_pass -h$shippinglite4_ro_ip shipping_lite_${shard} >>$CWD/output4.csv
 echo "$Query" | mysql -N -u$decoded_shippinglite5_user -p$decoded_shipping_lite_5_pass -h$shippinglite5_ro_ip shipping_lite_${shard} >>$CWD/output5.csv
 echo "$Query" | mysql -N -u$decoded_shippinglite6_user -p$decoded_shipping_lite_6_pass -h$shippinglite6_ro_ip shipping_lite_${shard} >>$CWD/output6.csv

 echo "$Query" |mysql -N  -u$shipping_lite_1_ws_user -p$shipping_lite_1_ws_pwd -h$shipping_lite_1_ws_host --port=4000 shipping_lite_1_ws_db >> $CWD/lite_w1.csv
  echo "$Query" |mysql -N  -u$shipping_lite_2_ws_user -p$shipping_lite_2_ws_pwd -h$shipping_lite_2_ws_host --port=4000 shipping_lite_2_ws_db >> $CWD/lite_w2.csv
   echo "$Query" |mysql -N  -u$shipping_lite_3_ws_user -p$shipping_lite_3_ws_pwd -h$shipping_lite_3_ws_host --port=4000 shipping_lite_3_ws_db >> $CWD/lite_w3.csv
    echo "$Query" |mysql -N  -u$shipping_lite_4_ws_user -p$shipping_lite_4_ws_pwd -h$shipping_lite_4_ws_host --port=4000 shipping_lite_4_ws_db >> $CWD/lite_w4.csv
     echo "$Query" |mysql -N  -u$shipping_lite_5_ws_user -p$shipping_lite_5_ws_pwd -h$shipping_lite_5_ws_host --port=4000 shipping_lite_5_ws_db >> $CWD/lite_w5.csv
      echo "$Query" |mysql -N  -u$shipping_lite_6_ws_user -p$shipping_lite_6_ws_pwd -h$shipping_lite_6_ws_host --port=4000 shipping_lite_6_ws_db >> $CWD/lite_w6.csv


      awk -F, '{ $2 = "shard1_" $2 }1' OFS=, $CWD/output1.csv >$CWD/shard1.csv
      awk -F, '{ $2 = "shard2_" $2 }1' OFS=, $CWD/output2.csv >$CWD/shard2.csv
      awk -F, '{ $2 = "shard3_" $2 }1' OFS=, $CWD/output3.csv >$CWD/shard3.csv
      awk -F, '{ $2 = "shard4_" $2 }1' OFS=, $CWD/output4.csv >$CWD/shard4.csv
      awk -F, '{ $2 = "shard5_" $2 }1' OFS=, $CWD/output5.csv >$CWD/shard5.csv
      awk -F, '{ $2 = "shard6_" $2 }1' OFS=, $CWD/output6.csv >$CWD/shard6.csv

      awk -F, '{ $2 = "shard1_" $2 }1' OFS=, $CWD/lite_w1.csv >$CWD/shard7.csv
      awk -F, '{ $2 = "shard2_" $2 }1' OFS=, $CWD/lite_w2.csv >$CWD/shard8.csv
      awk -F, '{ $2 = "shard3_" $2 }1' OFS=, $CWD/lite_w3.csv >$CWD/shard9.csv
      awk -F, '{ $2 = "shard4_" $2 }1' OFS=, $CWD/lite_w4.csv >$CWD/shard10.csv
      awk -F, '{ $2 = "shard5_" $2 }1' OFS=, $CWD/lite_w5.csv >$CWD/shard11.csv
      awk -F, '{ $2 = "shard6_" $2 }1' OFS=, $CWD/lite_w6.csv >$CWD/shard12.csv



       cat $CWD/shard*.csv > $CWD/final.csv

       (echo -e "Hey everyone,\nPls find the large ext shipments that got rtoed post 5 pickup reattempts"; uuencode $CWD/final.csv $CWD/final.csv ) | mailx -a "From: $MAILFROM" -s "$SUBJECT" $MAILTO


       rm $CWD/output*.csv $CWD/shard*.csv $CWD/lite_w*.csv
